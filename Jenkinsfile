pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "your_dockerhub_username"
        IMAGE_NAME     = "testjenkins-app"
        IMAGE_TAG      = "${env.BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-cred',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                    docker push $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        success {
            echo "Image pushed: $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG"
        }
        failure {
            echo "Build or push failed"
        }
        always {
            sh 'docker logout'
        }
    }
}
