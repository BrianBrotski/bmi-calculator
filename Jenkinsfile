pipeline {
    agent any

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