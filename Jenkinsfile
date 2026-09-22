pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'islembellil1'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Code récupéré depuis GitHub'
            }
        }

        stage('Build des images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Push vers Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                 usernameVariable: 'DH_USER',
                                 passwordVariable: 'DH_PASS')]) {
                    sh 'echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin'
                    sh 'docker compose push backend frontend'
                }
            }
        }

        stage('Déploiement') {
            steps {
                sh 'docker compose down || true'
                sh 'docker compose up -d'
                sh 'docker ps'
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
        success {
            echo 'Application disponible sur http://192.168.33.10:4200'
        }
    }
}
