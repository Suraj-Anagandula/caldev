pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "surajanagandula"
        IMAGE_NAME = "caldev"
        TAG = "latest"
    }

    stages {

        stage('Clone') {
            steps {
                     checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$TAG .'
            }
        }

        stage('Login DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $DOCKERHUB_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKERHUB_USER/$IMAGE_NAME:$TAG'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop calc || true
                docker rm calc || true
                docker run -d -p 3000:3000 --name calc $DOCKERHUB_USER/$IMAGE_NAME:$TAG
                '''
            }
        }
    }

    post {
        success {
            echo 'Calculator App Deployed Successfully 🚀'
        }
        failure {
            echo 'Pipeline Failed ❌'
        }
    }
}
