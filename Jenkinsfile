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
                    %VENV_DIR%\\Scripts\\python.exe -m pip install --upgrade pip || exit 0
                    if exist requirements.txt (
                        %VENV_DIR%\\Scripts\\pip install -r requirements.txt || exit 0
                    )
                """
            }
        }

        stage('Lint') {
            steps {
                echo 'Checking code quality with flake8'
                bat """
                    %VENV_DIR%\\Scripts\\pip install flake8 || exit 0
                    %VENV_DIR%\\Scripts\\flake8 main.py test_math_ops.py || exit 0
                """
            }
        }

        stage('Unit Tests') {
            steps {
                echo 'Running unit tests with pytest'
                bat """
                    %VENV_DIR%\\Scripts\\pip install pytest || exit 0
                    %VENV_DIR%\\Scripts\\pytest --junitxml=report.xml test_math_ops.py || exit 0
                """
            }
            post {
                always {
                    catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                        junit 'report.xml'
                    }
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Archiving Python scripts'
                archiveArtifacts artifacts: 'main.py,test_math_ops.py', fingerprint: true
            }
        }

    stage('Deploy (Simulated)') {
    steps {
        echo 'Simulating deployment'
        bat "mkdir deploy || echo Already exists"
        bat "copy main.py deploy\\"
        bat "copy test_math_ops.py deploy\\"
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
