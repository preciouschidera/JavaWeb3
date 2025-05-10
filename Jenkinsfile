pipeline {
    agent any

    environment {
        PATH = "/usr/bin:$PATH"
        DOCKERHUB_CREDENTIALS = credentials('leocrita')
    }

    stages {
        stage('Debug PATH') {
            steps {
                sh 'echo Current PATH: $PATH'
                sh 'which docker'
                sh 'docker --version || echo "Docker not found"'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t leocrita/java-web-calculator .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'leocrita', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh '''
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push leocrita/java-web-calculator
                    '''
                }
            }
        }
    }
}

