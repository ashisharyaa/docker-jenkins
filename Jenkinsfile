pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('137ae76e-b8ea-41ca-b08f-bf7978208ba2')
        IMAGE_NAME = 'mydocker0707/webapp'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    def dockerImage

                    // Build Docker image
                    dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}", "--file Dockerfile .")

                    // Authenticate with Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        // Push the Docker image to Docker Hub
                        dockerImage.push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image build and push successful!'
        }

        failure {
            echo 'Docker image build and push failed!'
        }
    }
}