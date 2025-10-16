pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPO = '484395054942.dkr.ecr.ap-south-1.amazonaws.com/sample-app'
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
        stage('Deploy Container') {
            steps {
                script {
                    echo "Deploying Docker container..."
                    sh '''
                        # Stop and remove existing container if it’s running
                        docker rm -f sample-app-container || true

                        # Pull the latest image from ECR
                        docker pull 484395054942.dkr.ecr.ap-south-1.amazonaws.com/sample-app:latest

                        # Run the container on port 3000
                        docker run -d --name sample-app-container -p 3000:3000 484395054942.dkr.ecr.ap-south-1.amazonaws.com/sample-app:latest
                    '''
                }
            }
        }
    }
}
