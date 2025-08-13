pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Pathum-Dilhara/CICD-Jenkins-Docker'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t pathumdilhara/reactapp:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'dockerhub-password')]) {
                    script {
                        bat "docker login -u pathumdilhara -p %dockerhub-password%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push pathumdilhara/reactapp:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}