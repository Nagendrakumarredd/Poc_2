pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = 'syamalanagendrakumarreddy'
        IMAGE_NAME      = 'my-app'
        IMAGE_TAG       = 'latest'
    }
 
    stages {
 
        stage('Fetch Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Nagendrakumarredd/Poc_2.git'
            }
        }
 
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }
 
        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-creds',
                        usernameVariable: 'syamalanagendrakumarreddy',
                        passwordVariable: 'Docker@123'
                    )
                ]) {
                        sh '''#!/bin/bash
                                        set +x
                                        echo "$Docker@123" | docker login -u "syamalanagendrakumarreddy" --password-stdin
                                        docker push syamalanagendrakumarreddy/my-app:latest
                                    '''
 
                }
            }
        }
 
        stage('Deploy to Client (Local Run)') {
            steps {
                sh "docker rm -f my-app-container || true"
                sh "docker run -d -p 80:80 --name my-app-container ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }
}
