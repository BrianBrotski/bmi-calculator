pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SonarToken')
    }

    stages{
        stage('Checkout') {
            steps{
                git 'https://github.com/brian.brotski/bmi-calculator.git'
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