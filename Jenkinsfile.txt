pipeline {
    agent any

    environment {
        PATH = "/usr/bin:$PATH"
        DOCKERHUB_CREDENTIALS = credentials('johnsonbv-creds-id')
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
                sh 'docker build -t johnsonbv/java-web-calculator .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'johnsonbv-creds-id', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh '''
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push johnsonbv/java-web-calculator
                    '''
                }
            }
        }
    }
}

