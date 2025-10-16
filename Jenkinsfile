pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPO = '484395054942.dkr.ecr.ap-south-1.amazonaws.com/simple-app'
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/praveenKumar-112/sample-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t sample-app .'
                }
            }
        }

        stage('Tag & Push to AWS ECR') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    script {
                        sh '''
                            aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                            aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                            aws configure set default.region ${AWS_REGION}

                            aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO}

                            docker tag sample-app:latest ${ECR_REPO}:${IMAGE_TAG}
                            docker push ${ECR_REPO}:${IMAGE_TAG}
                        '''
                    }
                }
            }
        }
    }
}
