
pipeline {
    agent any

    environment {
        REGISTRY   = "localhost:5000"
        IMAGE_NAME = "student-demo-app"
        PYTHON     = "C:/Users/STUDENT/AppData/Local/Programs/Python/Python313/python.exe"
    }

    stages {
        stage('1. Application Build & Test') {
            steps {
                echo "Verifying Python installation..."
                bat '"%PYTHON%" --version'

                echo "Verifying Python syntax..."
                bat '"%PYTHON%" -m py_compile app.py'
            }
        }

        stage('2. Build & Tag Docker Image') {
            steps {
                echo "Building Docker image..."

                bat """
                    docker build -t ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} -t ${REGISTRY}/${IMAGE_NAME}:latest .
                """
            }
        }

        stage('3. Push Image to Registry') {
            steps {
                echo "Pushing images to local registry..."

                bat "docker push ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"

                bat "docker push ${REGISTRY}/${IMAGE_NAME}:latest"
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded!"
            echo "Image: ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
        }

        failure {
            echo "Pipeline failed. Check the console output."
        }
    }
}
