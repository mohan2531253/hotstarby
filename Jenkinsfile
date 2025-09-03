pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'mohan2366'     // Docker Hub username
        IMAGE_NAME = 'hotstar'            // Docker Hub repository name
        IMAGE_TAG = 'v1'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'mohan2531253', 
                    url: 'https://github.com/mohan2531253/hotstarby.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                      credentialsId: 'docker',  
                      usernameVariable: 'DOCKER_USER',
                      passwordVariable: 'DOCKER_PASS'
                   )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $DOCKER_HUB_USER/$IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }

        stage('Docker Swarm Deploy') {
            steps {
                sh '''
                docker service rm hotserv || true
                docker service create \
                  --name hotserv \
                  -p 8008:8080 \
                  --replicas 3 \
                  $DOCKER_HUB_USER/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
