pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'emnadammak/mon-app'
        HELM_CHART_PATH = './mon-app'
    }
    stages {
        stage('Cloner le dépôt') {
            steps {
                git 'https://github.com/emna-dammak/Kubernetes-Pipeline'
            }
        }
        stage('Construire l\'image Docker') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Pousser l\'image Docker') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
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
