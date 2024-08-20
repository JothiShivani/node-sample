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
          // Run the Docker container in detached mode (-d)
                    def container = docker.image("${DOCKER_IMAGE}:latest").run('-d')

                    // Execute commands inside the running container
                    bat "docker exec ${container.id} echo 'Hello from inside Docker'"
                    bat "docker exec ${container.id} npm install"
                    bat "docker exec ${container.id} npm test"

                    // Stop and remove the container
                    container.stop()
                    bat "docker rm ${container.id}"
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
