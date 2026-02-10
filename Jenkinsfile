pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'wedding-website'
        DOCKER_TAG = 'latest'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }
        
        stage('Deploy to Server') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'server-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        sshpass -p '${PASSWORD}' scp -o StrictHostKeyChecking=no docker-compose.yml ${USERNAME}@your-server-ip:/path/to/deployment/
                        sshpass -p '${PASSWORD}' ssh -o StrictHostKeyChecking=no ${USERNAME}@your-server-ip 'cd /path/to/deployment && docker-compose pull && docker-compose up -d'
                    """
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
} 