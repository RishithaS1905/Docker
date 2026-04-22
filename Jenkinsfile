pipeline {
    agent any

    environment {
        // Define variables to use throughout the pipeline
        DOCKER_IMAGE = "myapp"
        DOCKER_HUB_USER = "rishithas1905" // Replace with your actual Docker Hub username
    }

    stages {
        stage('Checkout') {
            steps {
                // This clones the 'main' branch of your repo
                git branch: 'main', url: 'https://github.com/RishithaS1905/Docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Building the image using the local Dockerfile
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                // Ensure you have a 'docker-hub-credentials' ID set up in Jenkins Credentials
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Tag and push to Docker Hub
                    sh "docker tag ${DOCKER_IMAGE} ${DOCKER_HUB_USER}/${DOCKER_IMAGE}:latest"
                    sh "docker push ${DOCKER_HUB_USER}/${DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully! Image ${DOCKER_IMAGE} pushed."
        }
        failure {
            echo "Pipeline failed. Check the console output for errors."
        }
    }
}
