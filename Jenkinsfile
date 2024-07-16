pipeline {
    agent any

    stages {
        
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    npm install serve
                    ls -la
                '''
            }
        }
        stage('Test') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    (ls build/index.html >> /dev/null 2>&1 && return 0) || return 1
                    npm test
                '''
            }
        }
        stage('E2E') {
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.45.1-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    node_modules/.bin/serve -s build &
                    sleep 10
                    sudo npx playwright test --reporter=html
                '''
            }
        }
    }
}
