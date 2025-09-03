pipeline {
    agent any

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
                docker rmi -f hotstar:v1 || true
                docker build -t hotstar:v1 -f Dockerfile .
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f hotstar || true
                docker run -d --name hotstar -p 9090:8080 hotstar:v1
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
