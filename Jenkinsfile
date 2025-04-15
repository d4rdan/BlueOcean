pipeline {
    agent any
    
    environment {
        // Update these variables with your information
        DOCKER_HUB_CREDS = credentials('docker-hub-credentials') // Jenkins credentials ID
        DOCKER_IMAGE_NAME = 'yourdockerhubusername/demo-app'     // Update with your Docker Hub username
        DOCKER_IMAGE_TAG = "${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG .'
                sh 'docker tag $DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG $DOCKER_IMAGE_NAME:latest'
            }
        }
        
        stage('Push Docker Image') {
            steps {
                sh 'echo $DOCKER_HUB_CREDS_PSW | docker login -u $DOCKER_HUB_CREDS_USR --password-stdin'
                sh 'docker push $DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG'
                sh 'docker push $DOCKER_IMAGE_NAME:latest'
            }
        }
    }
    
    post {
        always {
            sh 'docker logout'
        }
    }
}
