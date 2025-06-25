pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build (übersprungen)') {
            steps {
                echo '⚠️ Build wird in dieser Jenkins-Umgebung nicht ausgeführt.'
                echo '📦 Du kannst npm run build lokal ausführen.'
            }
        }
        stage('Fertig') {
            steps {
                echo '✅ Jenkins hat den Code erfolgreich aus dem Repository geladen.'
            }
        }
    }
}
