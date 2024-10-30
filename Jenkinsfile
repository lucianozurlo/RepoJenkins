pipeline {
    environment {
        IMAGEN = "lucianozurlo/mi-nginx" 
        DOCKER_CREDENTIALS_ID = 'dockerhub-id' 
    }
    agent any
    stages {
        stage('Clone') {
            steps {
                script {
                    git branch: "main", url: 'https://github.com/lucianozurlo/Repojenkins.git'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    newApp = docker.build "${env.IMAGEN}:${env.BUILD_NUMBER}"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("${env.IMAGEN}:${env.BUILD_NUMBER}").inside('-u root') {
                        sh 'apache2ctl -v' 
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('', env.DOCKER_CREDENTIALS_ID) {
                        newApp.push()
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh "docker rmi ${env.IMAGEN}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}