pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-node-app"
    }

    stages {
        stage('Checkout') {
            steps {

                git branch: 'main', url: 'https://github.com/JothiShivani/node-sample.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:latest").inside {
                
                        bat 'echo "Hello from inside Docker"'
                        bat 'npm install'
                        bat 'npm test' 
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                  
                    docker.image("${DOCKER_IMAGE}:latest").run('-p 3005:3005')
                }
            }
        }

        stage('Cleanup') {
            steps {
                
                bat 'docker system prune -f'
            }
        }
    }

    post {

        success {
           
            echo 'Build and deploy succeeded!'
        }

        failure {
           
            echo 'Build or deploy failed.'
        }
    }
}
