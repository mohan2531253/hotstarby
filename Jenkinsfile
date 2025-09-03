pipeline {
    agent any

    environment {
        IMAGE_NAME = "mohan2366/hotstar-app"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'your-credentials-id',
                    url: 'https://github.com/mohan2531253/hotstarby.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f $IMAGE_NAME:$IMAGE_TAG || true
                    docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                    sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f hotstar || true
                    docker run -d --name hotstar -p 9090:8080 $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Docker Swarm Deploy') {
            steps {
                echo 'Swarm deployment step (if needed)'
            }
        }
    }
}
