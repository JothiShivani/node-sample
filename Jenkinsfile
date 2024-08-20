pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "joeshiv/my-node-app:latest"  // Update with your Docker Hub username
        DOCKER_CREDENTIALS_ID = 'DOCKERPASS'  // Make sure this matches the ID in Jenkins credentials
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
            withDockerRegistry([url: 'https://index.docker.io/v1/', credentialsId: DOCKER_CREDENTIALS_ID]) {
                bat 'docker push joeshiv/my-node-app:latest --max-attempts=10'
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
