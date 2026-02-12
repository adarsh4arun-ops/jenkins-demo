pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        PYTHON_EXE = 'C:/Users/student/AppData/Local/Programs/Python/Python312/python.exe'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling code from Git'
                checkout scm
            }
        }

        stage('Setup Environment') {
            steps {
                echo 'Setting up Python virtual environment'
                bat """
                    %PYTHON_EXE% -m venv %VENV_DIR%
                    %VENV_DIR%\\Scripts\\pip install --upgrade pip
                    if exist requirements.txt (
                        %VENV_DIR%\\Scripts\\pip install -r requirements.txt
                    )
                """
            }
        }

        stage('Lint') {
            steps {
                echo 'Checking code quality with flake8'
                bat """
                    %VENV_DIR%\\Scripts\\pip install flake8
                    %VENV_DIR%\\Scripts\\flake8 calculator tests
                """
            }
        }

        stage('Unit Tests') {
            steps {
                echo 'Running unit tests with pytest'
                bat """
                    %VENV_DIR%\\Scripts\\pip install pytest
                    %VENV_DIR%\\Scripts\\pytest --junitxml=report.xml
                """
            }
            post {
                always {
                    junit 'report.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Archiving Python scripts'
                archiveArtifacts artifacts: '**/*.py', fingerprint: true
            }
        }

        stage('Deploy (Simulated)') {
            steps {
                echo 'Simulating deployment'
                bat "mkdir deploy || echo Already exists"
                bat "xcopy calculator deploy /E /Y"
                bat "xcopy tests deploy /E /Y"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! ✅'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
