
pipeline {
    agent any

    environment {
        REGISTRY   = "localhost:5000"
        IMAGE_NAME = "student-demo-app"

        PYTHON = "C:/Users/STUDENT/AppData/Local/Programs/Python/Python313/python.exe"

        DOCKER = "C:/Users/STUDENT/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe"
    }

    stages {

        stage('1. Application Build & Test') {
            steps {
                echo "Checking Python installation..."
                bat '"%PYTHON%" --version'

                echo "Checking Python syntax..."
                bat '"%PYTHON%" -m py_compile app.py'
            }
        }

        stage('2. Docker Verification') {
            steps {
                echo "Checking Docker..."
                bat '"%DOCKER%" --version'
                bat '"%DOCKER%" info'
            }
        }

        stage('3. Build & Tag Docker Image') {
            steps {
                echo "Building Docker image..."

                bat '''
                    "%DOCKER%" build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% -t %REGISTRY%/%IMAGE_NAME%:latest .
                '''
            }
        }

        stage('4. Push Image to Registry') {
            steps {
                echo "Pushing versioned image..."

                bat '"%DOCKER%" push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%'

                echo "Pushing latest image..."

                bat '"%DOCKER%" push %REGISTRY%/%IMAGE_NAME%:latest'
            }
        }
    }

    post {
        success {
            echo "SUCCESS! Jenkins CI/CD Pipeline completed."
            echo "Image: ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
        }

        failure {
            echo "Pipeline failed. Check console output."
        }
    }
}
