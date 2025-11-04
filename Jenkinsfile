pipeline {
    agent any

    environment {
        IMAGE_NAME = "calculator-app"
        CONTAINER_NAME = "calculator-container"
        PORT = "5000"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo '📦 Cloning repository...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '🚀 Deploying container...'
                sh '''
                # Stop and remove old container if it exists
                if [ "$(docker ps -aq -f name=$CONTAINER_NAME)" ]; then
                    docker rm -f $CONTAINER_NAME
                fi

                # Run the new container
                docker run -d -p $PORT:5000 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! Application running on port 5000.'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
