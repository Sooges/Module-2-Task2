pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Sooges/Module-2-Task2'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(‘Dockerfile)
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh "docker run -d -p 80:80 Dockerfile"
                }
            }
        }
    }
}
