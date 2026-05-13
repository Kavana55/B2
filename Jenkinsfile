pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds'
        APP_NAME = "kavana55/my-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${APP_NAME}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Login and Push') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: "${DOCKERHUB_CREDENTIALS}",
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh '''
                    echo "$PASS" | docker login -u "$USER" --password-stdin
                    '''

                    sh "docker push ${APP_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
}
