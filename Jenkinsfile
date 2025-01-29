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
                    docker.build("frontend", "./frontend")
                    docker.build("backend", "./backend")
                    docker.build("database", "./database")
                }
            }
        }
        stage('Push Images to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                        docker.image("frontend").push("latest")
                        docker.image("backend").push("latest")
                        docker.image("database").push("latest")
                    }
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
    post {
        always {
            echo 'Pipeline Complete!'
        }
    }
}
