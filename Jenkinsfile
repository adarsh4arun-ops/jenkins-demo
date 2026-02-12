pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Python Script') {
            steps {
                sh 'python3 main.py'
            }
        }
    }

    post {
        success {
            echo 'Python script executed successfully'
        }
        failure {
            echo 'Execution failed'
        }
    }
}

