pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            // reuseNode true is helpful if you have specific workspace requirements
            reuseNode true 
        }
    }

    stages {
        stage("Build") {
            steps {
                sh '''
                ls -la
                npm --version
                node --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }
    
        stage("Test") {
            steps {
                sh '''
                test -f src/index.css
                npm test
                '''
            }
        }

        stage("E2E") {

            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.57.0-noble'
                    reuseNode true
                }

            }
            steps {
                sh '''
                npm install -g serve
                serve -s build
                npm playwright test
                '''
            }
        }

    } 

    post {
        always {
           
            junit 'test-results/junit.xml'
        }
    }
}