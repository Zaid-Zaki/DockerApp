pipeline {
    agent {
        label 'ubuntu-latest' // Use an agent with an Ubuntu environment
    }

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') // Ensure you create a Docker Hub credential in Jenkins with ID 'docker-hub-credentials'
        DOCKER_IMAGE = 'salarymodel'
        DOCKER_TAG = 'zaid056/first-action-docker-project:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    sh "docker build . -t ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                // Login to Docker Hub using credentials stored in Jenkins
                script {
                    sh "echo ${DOCKER_HUB_CREDENTIALS_PSW} | docker login -u ${DOCKER_HUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                // Tag the Docker image
                script {
                    sh "docker tag ${DOCKER_IMAGE}:latest ${DOCKER_TAG}"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the tagged Docker image to Docker Hub
                script {
                    sh "docker push ${DOCKER_TAG}"
                }
            }
        }
    }

    post {
        always {
            // Clean up after the build process
            sh "docker rmi ${DOCKER_IMAGE}:latest || true"
            sh "docker rmi ${DOCKER_TAG} || true"
        }
        success {
            echo 'Build, tag, and push successful!'
        }
        failure {
            echo 'Something went wrong during the process!'
        }
    }
}
