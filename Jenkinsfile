pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        EC2_USER = 'ubuntu'
        EC2_IP = '13.53.129.200'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git 'https://github.com/Sooges/Module-2-Task2'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def customImage = docker.build("my-docker-image:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        customImage.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def remote = [:]
                    remote.name = "EC2"
                    remote.host = "${env.EC2_IP}"
                    remote.user = "${env.EC2_USER}"
                    remote.identityFile = "${env.SSH_KEY}"
                    remote.allowAnyHosts = true

                    sshCommand remote: remote, command: """
                        docker pull my-docker-image:${env.BUILD_ID}
                        docker stop my_container || true
                        docker rm my_container || true
                        docker run -d --name my_container -p 80:80 my-docker-image:${env.BUILD_ID}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
    }
}
