pipeline {
    agent any  // Runs on any available agent

    environment {
        application = "addressbook"
        dockerhubaccountid = "jenkinsdemo123"
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm  // Checkout source code from version control
            }
        }

        stage('Build image') {
            steps {
                script {
                    // Build Docker image
                    def app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    // Push Docker image to DockerHub
                    withDockerRegistry([credentialsId: "dockerHub", url: ""]) {
                        app.push()
                        app.push("latest")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy Docker container
                    sh "docker run -d -p 81:8080 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Remove old images') {
            steps {
                script {
                    // Remove old Docker images
                    sh "docker rmi ${dockerhubaccountid}/${application}:latest -f"
                }
            }
        }
    }
}
