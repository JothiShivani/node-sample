pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "jothishivani/my-node-app:latest"  // Updated with your Docker Hub username
        DOCKER_CREDENTIALS_ID = 'docker-hub-credential'  // Updated to match your credentials ID
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    bat 'docker build -t %DOCKER_IMAGE% .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run Docker container in detached mode
                    bat 'docker run -d -p 3005:3005 --name my_container %DOCKER_IMAGE%'
                    
                    // Wait for the container to start
                    sleep(time: 10, unit: 'SECONDS')
                    
                    // Check container logs
                    bat 'docker logs my_container'
                    
                    // Run any tests against the container (customize this as needed)
                    bat 'curl http://localhost:3005'
                    
                    // Stop and remove the container
                    bat 'docker stop my_container'
                    bat 'docker rm my_container'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([url: "https://index.docker.io/v1/", credentialsId: DOCKER_CREDENTIALS_ID]) {
                        //bat tag cli jothishivani/cli
                        //docker push jothishivani/cli(to dockerhub)
                        bat 'docker push %DOCKER_IMAGE%'
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                cleanWs() // Clean up workspace
            }
        }
    }
}
