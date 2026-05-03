pipeline {
    agent any

    environment {
        DOCKER_USER = "richang9125"
    }

    stages {

        stage('Build Image') {
            steps {
                sh 'docker build -t healthcare-app .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag healthcare-app $DOCKER_USER/healthcare-app'
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                sh 'trivy image --exit-code 1 --severity HIGH,CRITICAL $DOCKER_USER/healthcare-app'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker push $DOCKER_USER/healthcare-app'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
