pipeline {
    agent any

    environment {
        IMAGE_NAME = "calculator-app"
        CONTAINER_NAME = "calculator-container"
        PORT = "3000"
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
                echo "🧹 Cleaning up old containers..."
                # Stop and remove old container if it exists
                if [ "$(docker ps -aq -f name=$CONTAINER_NAME)" ]; then
                    docker rm -f $CONTAINER_NAME
                fi

                echo "🏃‍♂️ Starting new container with auto-restart enabled..."
                # Run new container with restart policy
                docker run -d \
                    --restart unless-stopped \
                    -p $PORT:3000 \
                    --name $CONTAINER_NAME \
                    $IMAGE_NAME

                echo "✅ Container started successfully."
                docker ps --filter "name=$CONTAINER_NAME"
                '''
            }
        }

        stage('Check Application Health') {
            steps {
                echo '🔍 Checking if application is running...'
                sh '''
                sleep 5
                if curl -s http://localhost:$PORT > /dev/null; then
                    echo "✅ Application is responding on port $PORT."
                else
                    echo "⚠️ Application did not respond on port $PORT."
                    exit 1
                fi
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! Application is up and will auto-restart on reboot.'
        }
        failure {
            echo '❌ Deployment failed! Check Docker logs or Jenkins console for details.'
        }
    }
}
