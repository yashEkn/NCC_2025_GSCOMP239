pipeline {
    agent any

    environment {
        DOCKERHUB_NAMESPACE = "yashte"
        IMAGE_TAG = "${BUILD_NUMBER}"
        // Force traditional docker build instead of buildx
        DOCKER_BUILDKIT = "0"  // Disable BuildKit
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:yashEkn/NCC_2025_GSCOMP239.git',
                    credentialsId: 'git2'
            }
        }

        stage('Build Backend Docker image') {
            steps {
                script {
                    // Corrected build command with proper path spacing
                    sh "docker build -t ${DOCKERHUB_NAMESPACE}/backend-app:${IMAGE_TAG} ./backend"
                }
            }
        }

        stage('Build Frontend Docker image') {
            steps {
                script {
                    sh "docker build -t ${DOCKERHUB_NAMESPACE}/frontend-app:${IMAGE_TAG} ./frontend"
                }
            }
        }

        stage('Push Docker images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${DOCKERHUB_NAMESPACE}/backend-app:${IMAGE_TAG}"
                    sh "docker push ${DOCKERHUB_NAMESPACE}/frontend-app:${IMAGE_TAG}"
                    sh 'docker logout'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images to save disk space
            sh "docker rmi ${DOCKERHUB_NAMESPACE}/backend-app:${IMAGE_TAG} || true"
            sh "docker rmi ${DOCKERHUB_NAMESPACE}/frontend-app:${IMAGE_TAG} || true"
        }
    }
}
