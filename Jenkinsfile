pipeline {
    agent {
        docker{
            image 'node:14-alpine'
            args '-v /workspace:/workspace'
        }
    }

    environment {
        SONAR_TOKEN = credentials('sonarqube')
    }

    stages{
        stage('Checkout') {
            steps{
                git 'https://github.com/BrianBrotski/bmi-calculator.git'
            }
        }

        stage('Install Dependencies') {
            steps{
               script {
                  sh 'npm install'
               }
          }
        }

        stage('SonarQube Analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:latest'
                    args '-v /workspace:/workspace'
                }
            }
            steps{
                withSonarQubeEnv('SonarQube'){
                    sh """
                        sonar-scanner \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.projectKey=bmi-calculator \
                        -Dsonar.sources=. \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
                script {
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK'){
                        error "Pipeline aborted due to quality gate failure : ${qualityGate.status}"
                    }
                }
            }
        }

    }

    post{
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline succeeded'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}