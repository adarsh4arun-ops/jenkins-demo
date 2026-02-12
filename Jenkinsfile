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
                bat 'python app.py'
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
