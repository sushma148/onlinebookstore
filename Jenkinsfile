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
                sh 'docker build -t dockerlog123/onlinebookstore:latest .'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push dockerlog123/onlinebookstore:latest'
            }
        }
    }
}
