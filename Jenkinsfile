pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDS = credentials('dockerhub-credentials')
        DOCKER_IMAGE_NAME = "emnadammak/mon-app"
        // Ajout de ces variables pour s'assurer que Docker fonctionne correctement
        HELM_CHART_PATH = './mon-app'
        DOCKER_HOST = 'unix:///var/run/docker.sock'    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                checkout scm
            }
        }

        stage('Construire l\'image Docker') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE_NAME ."
                }
            }
        }

        stage('Pousser l\'image Docker') {
            steps {
                 withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    sh 'echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin'
                    sh "docker push ${DOCKER_IMAGE_NAME}"
                    sh "docker push ${DOCKER_IMAGE_NAME}:latest"
                }
            }
        }
       stage('Déployer avec Helm') {
            steps {
                  sh 'helm upgrade --install mon-app $HELM_CHART_PATH'
            }
        }
    }
}
