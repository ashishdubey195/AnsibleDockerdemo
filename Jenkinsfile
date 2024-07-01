pipeline {
    agent any  // Runs on any available agent

    environment {
        dockerhubaccountid = "jenkinsdemo123"
        application = "addressbook"
        BUILD_NUMBER = env.BUILD_NUMBER ?: "latest"  // Default to "latest" if BUILD_NUMBER is not available
    }

    stages {
        stage('Clone repository') {
            steps {
                // Checkout your source code repository (assuming SCM is configured)
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                script {
                    // Build Docker image using 'docker build' command
                    def dockerImageTag = "${dockerhubaccountid}/${application}:${BUILD_NUMBER}"
                    sh "docker build -t ${dockerImageTag} ."
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    // Push Docker image to Docker Hub or registry
                    def dockerImageTag = "${dockerhubaccountid}/${application}:${BUILD_NUMBER}"
                    sh "docker push ${dockerImageTag}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Example deployment command, adjust as per your deployment needs
                    def dockerImageTag = "${dockerhubaccountid}/${application}:${BUILD_NUMBER}"
                    sh "docker run -d -p 81:8080 -v /var/log/:/var/log/ ${dockerImageTag}"
                }
            }
        }

        stage('Remove old images') {
            steps {
                script {
                    // Remove old Docker images as needed
                    sh "docker rmi ${dockerhubaccountid}/${application}:${BUILD_NUMBER} -f"
                }
            }
        }
    }
}
