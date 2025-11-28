pipeline {
    agent any

    environment {
        DOCKERHUB_USER = credentials('dockerhub-username')
        DOCKERHUB_PASS = credentials('dockerhub-password')
        SERVER_SSH = credentials('ubuntu-server-ssh')  // ssh private key stored in Jenkins
        SERVER_IP = "YOUR.SERVER.IP"   // e.g., 54.201.123.10
        PROJECT_PATH = "/home/ubuntu/projecttask"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                sh """
                docker build -t ${DOCKERHUB_USER}/mean-backend ./backend
                """
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                sh """
                docker build -t ${DOCKERHUB_USER}/mean-frontend ./frontend
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh """
                echo ${DOCKERHUB_PASS} | docker login -u ${DOCKERHUB_USER} --password-stdin
                """
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                sh """
                docker push ${DOCKERHUB_USER}/mean-backend
                docker push ${DOCKERHUB_USER}/mean-frontend
                """
            }
        }

        stage('Deploy to VM') {
            steps {
                sshagent(['ubuntu-server-ssh']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} '
                        cd ${PROJECT_PATH} &&
                        docker-compose pull &&
                        docker-compose down &&
                        docker-compose up -d
                    '
                    """
                }
            }
        }
    }
}

