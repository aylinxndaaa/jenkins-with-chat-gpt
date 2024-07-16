pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-web-app'
        DOCKER_CONTAINER = 'my-web-app-container'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/aylinxndaaa/jenkins-with-chat-gpt.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.DOCKER_IMAGE}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    // Stop and remove the old container if it exists
                    sh "docker stop ${env.DOCKER_CONTAINER} || true"
                    sh "docker rm ${env.DOCKER_CONTAINER} || true"
                    
                    // Run the new container
                    dockerImage.run("-d -p 3000:3000 --name ${env.DOCKER_CONTAINER}")
                }
            }
        }
    }
}

