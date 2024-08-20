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
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run Docker container in detached mode
                    sh 'docker run -d -p 3005:3005 --name my_container ${DOCKER_IMAGE}'
                    
                    // Wait for the container to start
                    sleep(time: 10, unit: 'SECONDS')

                    // Check container logs
                    sh 'docker logs my_container'
                    
                    // Run any tests against the container (customize this as needed)
                    // Example: curl or wget commands to check if the application is running correctly
                    sh 'curl http://localhost:3005'

                    // Stop and remove the container
                    sh 'docker stop my_container'
                    sh 'docker rm my_container'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push Docker image to registry
                    sh 'docker push ${DOCKER_IMAGE}'
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
