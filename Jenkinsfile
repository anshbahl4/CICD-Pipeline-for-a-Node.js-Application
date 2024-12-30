pipeline {
  agent any

  environment {
    DOCKER_IMAGE = 'nodejs-ci-cd'
    DOCKER_TAG = 'latest'
  }

  stages {
    stage('Checkout') {
      steps {
        withCredentials([usernamePassword(credentialsId: '531268fe-91a2-473d-a58d-a2f5404221c2', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASSWORD')]) {
          sh """
          git clone https://${GIT_USER}:${GIT_PASSWORD}@github.com/anshbahl4/CICD-Pipeline-for-a-Node.js-Application.git .
          """
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
        }
      }
    }

    stage('Run Tests') {
      steps {
        script {
          docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").inside {
            sh 'npm install'
            sh 'npm test'
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: '531268fe-91a2-473d-a58d-a2f5404221c2', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
          script {
            docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_USER}:${DOCKER_PASSWORD}") {
              docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
            }
          }
        }
      }
    }
  }
}
