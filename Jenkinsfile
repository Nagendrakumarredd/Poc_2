pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'syamalanagendrakumarreddy'
        IMAGE_NAME      = 'my-app'
        IMAGE_TAG       = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Nagendrakumarredd/Poc_2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
    }
}
