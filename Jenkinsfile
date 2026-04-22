pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "myapp"
        DOCKER_HUB_USER = "rishithas1905" 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RishithaS1905/Docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Use 'bat' for Windows instead of 'sh'
                    bat "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    // Use 'bat' and wrap variables correctly for Windows
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    bat "docker tag ${DOCKER_IMAGE} ${DOCKER_HUB_USER}/${DOCKER_IMAGE}:latest"
                    bat "docker push ${DOCKER_HUB_USER}/${DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        success { echo "Success!" }
        failure { echo "Failed." }
    }
}
