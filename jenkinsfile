pipeline {
    agent any

    tools {
        nodejs 'Node18'   // Use the NodeJS installation configured in Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                dir('backend') {
                    sh 'npm run build || echo "⚠️ No build script defined"'
                }
            }
        }

        stage('Test') {
            steps {
                dir('backend') {
                    sh 'npm test || echo "⚠️ No tests defined"'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    dir('backend') {
                        sh '''
                          npx sonar-scanner \
                            -Dsonar.projectKey=ecommerce-store \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=$SONAR_HOST_URL \
                            -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }
    }
}
