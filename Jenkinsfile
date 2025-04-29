pipeline {
    agent any

    environment {
        IMAGE_NAME = "abhay202001/travel-website"
        DOCKER_CREDENTIALS_ID = "mukeshswami0612"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Skipping checkout - handled by Git SCM integration"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker rm -f travel-container || true'
                    sh 'docker run -d --name travel-container -p 80:80 ${IMAGE_NAME}:latest'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh 'docker push ${IMAGE_NAME}:latest'
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Docker image pushed to Docker Hub: ${IMAGE_NAME}:latest"
        }
        failure {
            echo "Something went wrong!"
        }
    }
}
