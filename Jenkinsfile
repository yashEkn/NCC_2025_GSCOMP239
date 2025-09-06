pipeline {
    agent any

    environment {
        DOCKERHUB_NAMESPACE = "yashte"
        IMAGE_TAG = "${BUILD_NUMBER}"  // unique tag for each Jenkins build
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:yashEkn/NCC_2025_GSCOMP239.git',
                    credentialsId: 'git2'   // your SSH key credential in Jenkins
            }
        }

        stage('Build Backend Docker image') {
            steps {
                script {
                    // Build backend image from ./backend folder
                    sh "docker build -t ${DOCKERHUB_NAMESPACE}/backend-app:${IMAGE_TAG} ./backend"
                }
            }
        }

        stage('Build Frontend Docker image') {
            steps {
                script {
                    // Build frontend image from ./frontend folder
                    sh "docker build -t ${DOCKERHUB_NAMESPACE}/frontend-app:${IMAGE_TAG} ./frontend"
                }
            }
        }

        stage('Push Docker images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    // Login to Docker Hub
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'

                    // Push images with BUILD_NUMBER as tag
                    sh "docker push ${DOCKERHUB_NAMESPACE}/backend-app:${IMAGE_TAG}"
                    sh "docker push ${DOCKERHUB_NAMESPACE}/frontend-app:${IMAGE_TAG}"

                    // Logout
                    sh 'docker logout'
                }
            }
        }
    }
}
