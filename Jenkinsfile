pipeline {
    agent any

    environment {
        REGISTRY = "docker.io/kranthizeroday"
        IMAGE_NAME = "inseccicd"
        
        // 🚨 SECURITY GAP: Hardcoded Docker Credentials
        DOCKER_USERNAME = "kranthizeroday"
        DOCKER_PASSWORD = "Vault1"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'git@github.com:Kranthi-cybsec7/inseccicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} ${REGISTRY}"
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.image("${REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                }
            }
        }

        stage('Run Application with Docker') {
            steps {
                script {
                    sh "docker run -d -p 5000:5000 ${REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}
