pipeline {
    agent any

    environment {
        // 1. EDIT: Change this to YOUR Docker Hub username
        DOCKER_IMAGE = "chandankumar69/my-jenkins-app"
        DOCKER_TAG = "v1"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                // 2. EDIT: Change 'dockerhub-credentials' to 'docker-hub-creds' 
                // (Matches the ID we created in Jenkins)
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop myapp-container || true
                docker rm myapp-container || true
                docker run -d -p 5000:5000 --name myapp-container $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    }
}
