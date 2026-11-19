
pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'master',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/oukNan/weather_app.git'
            }
        }

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
            steps {
                sh '''
                    echo "Initializing Node 18!"
                    echo "Weather App source code is ready."
                    ls -la
                '''
            }
        }

        /*
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
        */

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t weather-app:${BUILD_NUMBER} .
                '''
            }
        }

        
        stage('Inspect Docker Image') {
            steps {
                sh '''
                    docker run --rm weather-app:${BUILD_NUMBER} \
                    ls -la /usr/share/nginx/html/
                '''
            }
        }


        stage('Deploy Weather App') {
            steps {
                sh '''
                    docker rm -f weather-app || true

                    docker run -d \
                        --name weather-app \
                        -p 3000:80 \
                        weather-app:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true,
                  testResults: 'test-results/junit.xml'
        }
    }
}
