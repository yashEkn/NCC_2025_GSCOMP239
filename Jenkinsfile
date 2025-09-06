pipeline {
  agent any

  environment {
    DOCKERHUB_NAMESPACE = "yashte" 
    IMAGE_TAG = "latest"  // or use BUILD_NUMBER for unique tags
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'git@github.com:yashEkn/NCC_2025_GSCOMP239.git',
            credentialsId: 'git2'   // your SSH key credential in Jenkins
      }
    }

    stage('Build Docker image') {
      steps {
        script {
          sh "docker build -t yashte/gscomp239./backend"
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
          sh "docker push ${DOCKERHUB_NAMESPACE}/my-app:${IMAGE_TAG}"
          sh 'docker logout'
        }
      }
    }
  }
}
