pipeline {
    agent any

    environment {
        IMAGE_NAME = "rohidas9730/testquantum:latest"
        CONTAINER_NAME = "frontend-app"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'git-private-tocken', url: 'https://github.com/rohidas9730/TestQuantum.git'
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
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    }
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stop and remove existing container if running
                    sh 'docker stop $CONTAINER_NAME || true'
                    sh 'docker rm $CONTAINER_NAME || true'

                    // Run new container on port 80
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
