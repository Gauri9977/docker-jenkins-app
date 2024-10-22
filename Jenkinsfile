pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE = 'gauri9903/simple-node-app:9'
    }

    stages {
        stage('Clone Git Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Gauri9977/docker-jenkins-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
    }

    post {
        cleanup {
            cleanWs()
        }
    }
}
