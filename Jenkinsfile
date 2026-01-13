pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'test',
                    url: 'https://github.com/thanh-vq/testJenkins.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Build test branch'
            }
        }
    }
}
