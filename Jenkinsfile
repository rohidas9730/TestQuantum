pipeline {
    agent any

    environment {
        IMAGE_NAME = "rohidas9730/frontend-deployment:latest"
        CONTAINER_NAME = "frontend-app"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'git-private-tocken', url: 'https://github.com/your-username/frontend-deployment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker login -u rohidas9730 -p "rohidas@123"'
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stop and remove existing container
                    sh 'docker stop $CONTAINER_NAME || true'
                    sh 'docker rm $CONTAINER_NAME || true'

                    // Run new container
                    sh 'docker run -d --name $CONTAINER_NAME -p 80:80 $IMAGE_NAME'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
