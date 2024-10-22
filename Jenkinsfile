pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'  // Your Docker Hub credentials ID
        DOCKER_IMAGE = "yourdockerhubusername/simple-node-app" // Docker Hub image name
        GIT_REPO = "https://github.com/yourusername/docker-jenkins-app.git"  // Your GitHub repo URL
    }

    stages {
        stage('Clone Git Repo') {
            steps {
                git url: "${GIT_REPO}", branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh "docker rmi ${DOCKER_IMAGE}:${env.BUILD_ID}"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
