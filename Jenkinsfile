pipeline {
    agent any

    environment {
        APP_DIR = "/var/www/calculator"
        NODE_ENV = "production"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Jenkins will clone your repo automatically
                echo 'Code checked out.'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh """
                    sudo mkdir -p $APP_DIR
                    sudo cp -r * $APP_DIR
                    cd $APP_DIR
                    sudo npm install
                    sudo pm2 delete calculator || true
                    sudo pm2 start server.js --name calculator
                    sudo pm2 save
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
