pipeline {
  agent any

  environment {
    GIT_CREDENTIALS = credentials('531268fe-91a2-473d-a58d-a2f5404221c2')
    DOCKER_IMAGE = 'nodejs-ci-cd'
    DOCKER_TAG = 'latest'
  }

  stages {
    stage('Checkout') {
      steps {
        git credentialsId: "${GIT_CREDENTIALS}", url: 'https://github.com/anshbahl4/CICD-Pipeline-for-a-Node.js-Application.git'
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
        script {
          withDockerRegistry([credentialsId: "${GIT_CREDENTIALS}", url: 'https://index.docker.io/v1/']) {
            docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
          }
        }
      }
    }
  }
}
