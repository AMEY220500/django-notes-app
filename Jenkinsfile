@Library("Shared") _

pipeline {
    agent {label 'jenkins-agent'}
//    agent any
    stages {
        stage('Hello'){
            steps {
                script {
                    hello()
                }
            }
        }
        stage('Code') {
            steps {
                echo 'This is cloning the code'
                git url: 'https://github.com/AMEY220500/django-notes-app/', branch:'main'
                echo 'Code cloned successfully'
            }
        }
        stage('Build') {
            steps {
                echo 'This is building the code'
                sh 'docker build -t notes-app:latest .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                echo 'Pushing to Dockerhub'
                withCredentials([
                     usernamePassword(
                     credentialsId: 'docker-hub',
                     usernameVariable: 'DOCKER_USER',
                     passwordVariable: 'DOCKER_PASS'
                     )
                ]){
                sh "docker login -u ${env.DOCKER_USER} -p ${env.DOCKER_PASS}"
                sh "docker image tag notes-app:latest ${env.DOCKER_USER}/notes-app:latest"
                sh "docker push ${env.DOCKER_USER}/notes-app:latest"
                }
                  }
        }
        stage('Deploy') {
            steps {
                echo 'This is deploying the code'
                // sh 'docker compose pull'
                sh 'docker compose up -d'
            }
        }
    }
}
