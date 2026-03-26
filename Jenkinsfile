pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t calculator-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker stop calculator || true'
                sh 'docker rm calculator || true'
                sh 'docker run -d -p 3000:3000 --name caldev surajanagandula/caldev'
            }
        }
    }
}
