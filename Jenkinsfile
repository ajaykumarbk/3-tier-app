pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    sh 'sudo docker build -t frontend ./frontend'
                    sh 'sudo docker build -t backend ./backend'
                    sh 'sudo docker build -t database ./database'
                }
            }
        }
        stage('Push Images to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                        sh 'sudo docker image "frontend" push "latest"'
                        sh 'sudo docker image "backend" push "latest"'
                        sh 'sudo docker image "database" push "latest"'
                    }
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                sh 'sudo docker-compose down'
                sh 'sudo docker-compose up -d'
            }
        }
    }
    post {
        always {
            echo 'Pipeline Complete!'
        }
    }
}

