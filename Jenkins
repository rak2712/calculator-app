pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rak2712/calculator-app.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                // your build steps here
            }
        }
    }
}
