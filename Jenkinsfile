pipeline {
  agent any
  environment {
    IMAGE = "frontend-app:${env.BUILD_ID}"
    CONTAINER_NAME = "frontend-service"
    PORT = "80"
  }
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Build') {
      steps {
        sh 'npm ci'
        sh 'npm run build'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh "docker build -t ${IMAGE} ."
      }
    }
    stage('Deploy') {
      steps {
        sh """
          docker ps -q --filter "name=${CONTAINER_NAME}" | grep -q . && docker rm -f ${CONTAINER_NAME} || true
          docker run -d --name ${CONTAINER_NAME} -p ${PORT}:${PORT} ${IMAGE}
        """
      }
    }
  }
}


