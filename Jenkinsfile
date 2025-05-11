pipeline {

    agent any

    environment {

        DOCKER_HUB_CREDENTIALS = 'Dockerhub'

        IMAGE_NAME = 'leocrita/java-web-calculator'

    }

    stages {

        stage('Checkout') {

            steps {

                git branch: 'latest', url: 'https://github.com/preciouschidera/JavaWeb3.git'

            }

        }

        stage('Build Docker Image') {

            steps {

                script {

                    docker.build("${IMAGE_NAME}:latest")

                }

            }

        }

        stage('Push to Docker Hub') {

            steps {

                script {

                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_HUB_CREDENTIALS}") {

                        docker.image("${IMAGE_NAME}:latest").push()

                    }

                }

            }

        }

    }

}
 
