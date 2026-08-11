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

                    echo "=== DNS CONFIG ==="
                    cat /etc/resolv.conf

                    echo "=== NODE DNS ==="
                    node -e "require('dns').lookup('registry.npmjs.org', (err, address, family) => { console.log('err:', err); console.log('address:', address); console.log('family:', family); process.exit(err ? 1 : 0) })"

                    echo "=== NPM REGISTRY ==="
                    npm config get registry

                    echo "=== NPM CONNECTIVITY ==="
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