// // pipeline {
// //     agent any

// //     environment {
// //         DOCKER_IMAGE = "my-node-app"
// //     }

// //     stages {
// //         stage('Checkout') {
// //             steps {

// //                 git branch: 'main', url: 'https://github.com/JothiShivani/node-sample.git'
// //             }
// //         }

// //         stage('Build') {
// //             steps {
// //                 script {
// //                     docker.build("${DOCKER_IMAGE}:latest")
// //                 }
// //             }
// //         }

// //         stage('Test') {
// //     steps {
// //         script {
// //           // Run the Docker container in detached mode (-d)
// //                     def container = docker.image("${DOCKER_IMAGE}:latest").run('-d')

// //                     // Execute commands inside the running container
// //                     bat "docker ps -a"
// //                     bat "docker exec ${container} npm install"
// //                     bat "docker exec ${container} npm test"

// //                     // Stop and remove the container
// //                      bat "docker stop ${container}"
// //                     bat "docker rm ${container}"
// //         }
// //     }
// // }


// //         stage('Deploy') {
// //             steps {
// //                 script {
                  
// //                     docker.image("${DOCKER_IMAGE}:latest").run('-p 3005:3005')
// //                 }
// //             }
// //         }

// //         stage('Cleanup') {
// //             steps {
                
// //                 bat 'docker system prune -f'
// //             }
// //         }
// //     }

// //     post {

// //         success {
           
// //             echo 'Build and deploy succeeded!'
// //         }

// //         failure {
           
// //             echo 'Build or deploy failed.'
// //         }
// //     }
// // }

// pipeline {
//     agent any

//     environment {
//         DOCKER_IMAGE = "my-node-app:latest"
//         DOCKER_REGISTRY_CREDENTIALS_ID = "dockerhub-credentials-id" // Credentials ID for Docker Hub, if needed
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                git branch: 'main', url: 'https://github.com/JothiShivani/node-sample.git'
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 script {
//                     // Build the Docker image
//                     docker.build(DOCKER_IMAGE)
//                 }
//             }
//         }

//         stage('Run Tests') {
//             steps {
//                 script {
//                     // Run the Docker container in detached mode
//                     def container = docker.run(DOCKER_IMAGE, "-d", "-p", "8081:8081")
                    
//                     // Wait for the container to start
//                     sleep(time: 10, unit: 'SECONDS')

//                     // Check container logs
//                     sh "docker logs ${container.id}"
                    
//                     // Run any tests against the container (customize this as needed)
//                     // Example: curl or wget commands to check if the application is running correctly
//                     sh "curl http://localhost:8081"
                    
//                     // Stop the container
//                     sh "docker stop ${container.id}"
//                 }
//             }
//         }

//         stage('Push Docker Image') {
//             steps {
//                 script {
//                     docker.withRegistry('https://index.docker.io/v1/', DOCKER_REGISTRY_CREDENTIALS_ID) {
//                         docker.image(DOCKER_IMAGE).push('latest')
//                     }
//                 }
//             }
//         }
//     }

//     post {
//         always {
//             cleanWs()
//         }
//     }
// }
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-node-app:latest"
        DOCKER_REGISTRY_CREDENTIALS_ID = "dockerhub-credentials-id" // Replace with your Docker Hub credentials ID if needed
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
                    // Run the Docker container in detached mode, exposing port 8081
                    def container = docker.image(DOCKER_IMAGE).run('-d -p 8081:8081')
                    
                    // Wait for the container to initialize
                    bat 'timeout /t 10 /nobreak'
                    
                    // Check container logs for errors
                    bat "docker logs ${container.id}"
                    
                    // Perform a simple test to check if the application is accessible
                    bat "curl http://localhost:8081"
                    
                    // Stop the container after tests
                    bat "docker stop ${container.id}"
                    
                    // Remove the stopped container
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

        stage('Cleanup') {
            steps {
                script {
                    // Clean up any dangling Docker resources
                    sh 'docker system prune -f'
                }
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

        always {
            cleanWs()
        }
    }
}
