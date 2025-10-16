pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/praveenKumar-112/sample-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t sample-node-app .'
                }
            }
        }

        stage('Verify Image') {
            steps {
                script {
                    sh 'docker images'
                }
            }
        }
    }
}
