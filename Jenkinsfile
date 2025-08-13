pipeline {
    agent any
    
    stages {
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Pathum-Dilhara/CICD-Jenkins-Docker.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t pathumdilhara/reactapp:${BUILD_NUMBER} .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'dockerhub-password')]) {
                    script {  
                        sh "docker login -u pathumdilhara -p '${dockerhub-password}'"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                sh "docker push pathumdilhara/reactapp:${BUILD_NUMBER}"
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}