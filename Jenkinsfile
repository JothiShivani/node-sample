pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-node-app:latest"
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
                    // Example: using curl to check if the application is running correctly
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
                    // Push Docker image to registry
                    bat 'docker push %DOCKER_IMAGE%'
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
