pipeline {
    agent any

    stages {
            stage('Docker Network Test') {
                agent {
                    docker {
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }

                steps {
                    sh '''
                        echo "=== ENVIRONMENT ==="
                        node --version
                        npm --version

                        echo "=== DNS ==="
                        cat /etc/resolv.conf
                        getent hosts registry.npmjs.org

                        echo "=== NODE DNS ==="
                        node -e "require('dns').lookup('registry.npmjs.org', console.log)"

                        echo "=== NPM REGISTRY ==="
                        npm config get registry

                        echo "=== NPM TEST ==="
                        npm view yargs@16.2.0 version
                    '''
                }
            }
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
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
    }
}