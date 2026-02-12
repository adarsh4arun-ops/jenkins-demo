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
                bat 'C:/Users/student/AppData/Local/Programs/Python/Python312/python.exe main.py'
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
