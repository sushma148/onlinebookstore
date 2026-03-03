pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "dockerlog123/onlinebookstore"
        DOCKER_TAG = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/sushma148/onlinebookstore.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                usernameVariable: 'USERNAME',
                passwordVariable: 'PASSWORD')]) {

                    sh '''
                    echo $PASSWORD | docker login -u $USERNAME --password-stdin
                    docker push $DOCKER_IMAGE:$DOCKER_TAG
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ec2-user@YOUR_EC2_IP << EOF
                docker pull $DOCKER_IMAGE:$DOCKER_TAG
                docker stop app || true
                docker rm app || true
                docker run -d -p 80:80 --name app $DOCKER_IMAGE:$DOCKER_TAG
                EOF
                '''
            }
        }
    }
}
