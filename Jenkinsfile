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
                  docker build -t onlinebookstore:latest .
                  docker tag onlinebookstore:latest dockerlog123/onlinebookstore:latest
                '''
            }
        }

        stage('Push to Registry (Docker Hub)') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_PASS'
                )]) {
                    sh '''
                      echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                      docker push dockerlog123/onlinebookstore:latest
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                  docker stop bookstore || true
                  docker rm bookstore || true

                  docker pull dockerlog123/onlinebookstore:latest
                  docker run -d --name bookstore -p 8081:80 dockerlog123/onlinebookstore:latest

                  docker ps
                '''
            }
        }
    }
}
