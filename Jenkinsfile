pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:22.12-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    rm -rf node_modules
                    ls -la
                    npm install
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:22.12-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }
        stage('e2e') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.49.1-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 20
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'junit-results/junit.xml'
        }
    }
}
