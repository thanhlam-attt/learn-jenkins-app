pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '--dns=192.168.65.7' 
                    reuseNode true
                }
            }

            steps {
                sh '''
                    ls -la
                    echo 'Build Stage!'
                    node --version
                    npm --version
                    npm ci --verbose
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo 'Test Stage!'
                    test -f build/index.html
                '''
            }
        }
        stage('Test Again!') {
            steps {
                sh '''
                    echo 'Test Stage Again!'
                    ls -la
                '''
            }
        }
    }
}