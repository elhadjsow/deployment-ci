pipeline {
    agent any

    environment {
        DOCKER_HUB_IMAGE = 'elhadjsow/backend-certificat:latest'
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Clonage du code de déploiement...'
                checkout scm
            }
        }

        stage('Clean Old Containers') {
            steps {
                echo '🧹 Nettoyage des anciens conteneurs...'
                bat '''
                docker stop postgres_db backend_app backend_certificat 2>nul || exit /b 0
                docker rm postgres_db backend_app backend_certificat 2>nul || exit /b 0
                '''
            }
        }

        stage('Pull Latest Images') {
            steps {
                echo '🐳 Téléchargement des dernières images Docker...'
                bat '''
                docker pull %DOCKER_HUB_IMAGE%
                '''
            }
        }

        stage('Stop Current Stack') {
            steps {
                echo '⛔ Arrêt du stack actuel...'
                bat 'docker-compose down || exit /b 0'
            }
        }

        stage('Deploy Stack') {
            steps {
                echo '🚀 Déploiement du nouveau stack...'
                bat 'docker-compose up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '✅ Vérification du déploiement...'
                bat '''
                docker ps
                docker-compose logs
                '''
            }
        }
    }

    post {
        always {
            echo '📋 Pipeline de déploiement terminé'
        }
        success {
            echo '✅ Déploiement réussi!'
        }
        failure {
            echo '❌ Le déploiement a échoué'
        }
    }
}
