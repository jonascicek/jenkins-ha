pipeline {
    agent {
        docker {
            image 'node:18'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh '''
                    node -v
                    npm -v
                    npm ci
                    npm run build
                '''
            }
        }
    }
}
