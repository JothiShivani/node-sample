pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-node-app:latest"
        DOCKER_REGISTRY_CREDENTIALS_ID = "dockerhub-credentials-id" // Replace with your actual credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/JothiShivani/node-sample.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Run Tests') {
    steps {
        script {
            // Run the Docker container in detached mode
            def container = docker.run(DOCKER_IMAGE, "-d", "-p", "3005:3005")
            
            // Wait for the container to start
            bat 'timeout /t 10 /nobreak'

            // Check container logs
            bat "docker logs ${container.id}"
            
            // Run any tests against the container (customize this as needed)
            // Example: curl or wget commands to check if the application is running correctly
            bat "curl http://localhost:3005"

            // Stop and remove the container
            bat "docker stop ${container.id}"
            bat "docker rm ${container.id}"
        }
    }
}

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_REGISTRY_CREDENTIALS_ID) {
                        docker.image(DOCKER_IMAGE).push('latest')
                    }
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
