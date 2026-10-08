pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shreyavinay30/shreya-image"
        DOCKER_CREDS = "dockerhub-creds"
    }

    stages {
        stage('Build Application') {
            steps {
                echo 'Building application...'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image ${DOCKER_IMAGE}:latest..."
                    bat "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "${DOCKER_CREDS}",
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    script {
                        echo "Logging in to Docker Hub..."
                        bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                        
                        echo "Pushing image ${DOCKER_IMAGE}:latest..."
                        bat "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Image successfully built, tagged, and pushed to Docker Hub.'
        }
        failure {
            echo 'Pipeline failed. Check the logs above for errors.'
        }
    }
}
