pipeline {
    agent any

    stages {
        /*
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
        */
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u 0:0'
                }
            }
            steps{
                 sh'''
                        echo "initializing node 18!!"
                 '''
            }
        }
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.64.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    sleep 20
                    npx playwright test
                '''
            }
        }
        stage('Clone Repositories') {
            steps {
                git url: 'https://github.com/oukNan/weather_app.git',
                    branch: 'master'
            }
        }
    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
