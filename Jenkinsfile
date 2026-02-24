pipeline {
  agent any

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          echo "Building Docker image..."
          docker build -t onlinebookstore:latest .
        '''
      }
    }

    stage('Deploy Container') {
      steps {
        sh '''
          echo "Stopping old container..."
          docker stop bookstore || true
          docker rm bookstore || true

          echo "Starting new container..."
          docker run -d --name bookstore -p 8081:80 onlinebookstore:latest

          docker ps
        '''
      }
    }

  }
}
