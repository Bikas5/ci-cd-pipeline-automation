pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/ci-cd-pipeline-automation.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("your-username/multi-tenant-app:${env.BUILD_ID}")
                }
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest app/tests/'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
    post {
        always {
            sh 'kubectl get all'
        }
    }
}
