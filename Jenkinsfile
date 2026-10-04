pipeline {
    agent any

    environment {
        IMAGE = "ghcr.io/s-a-r-a123/training_task"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE}:latest .'
            }
        }

        stage('Login to GHCR') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'ghcr-credentials',
                        usernameVariable: 'GITHUB_USER',
                        passwordVariable: 'GITHUB_TOKEN'
                    )
                ]) {
                    sh '''
                        echo "$GITHUB_TOKEN" | docker login ghcr.io \
                        -u "$GITHUB_USER" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push ${IMAGE}:latest'
            }
        }
    }

    post {
        success {
            echo 'Docker image pushed successfully to GitHub Container Registry!'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}