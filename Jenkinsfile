pipeline {
    agent any

    env {
        NETLIFY_SITE_ID = '60799cac-f93c-447c-a100-dfae18140586'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')

    }

    stages {
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
                node --version
                npm --version
                npm ci
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
                test -f build/index.html
                npm test        
                '''
            }
            post {
               always {
                    junit 'test-results/junit.xml'
                }
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo "Deploy into production site ID: $NETLIFY_SITE_ID"
                node_modules/.bin/netlify status    
          '''
            }
        }
    }
}