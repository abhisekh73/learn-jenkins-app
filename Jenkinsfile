pipeline {
    agent any

    stages {
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.50.1-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve wait-on
                    npx serve -s build & 
                    npx wait-on http://localhost:3000  
                    npx playwright test
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
