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
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag mytomcat $DOCKER_USER/mytomcat:latest
                        docker push $DOCKER_USER/mytomcat:latest
                        docker logout
                    '''
                }
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
        stage('docker push'){
            steps{
                docker.withRegistry('https://index.docker.io/v1/','docker-hub-credentials'){
                    def app =
                        docker.build("mohan2366/hotstar-app:${env.BUILD_NUMBER}")
                    app.push()
             }  
         }
    }
}
