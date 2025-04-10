pipeline {
    agent any  // Runs the pipeline on any available agent

    environment {
        // You can define environment variables here if needed
    }

    stages {
        stage('Clone repository') {
            steps {
                // Checkout the repository
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                script {
                    // Build Docker image
                    app = docker.build("vladimirhristovski/KIII-H1")
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    // Push Docker image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        app.push("${env.BRANCH_NAME}-latest")
                    }
                }
            }
        }
    }

    post {
        always {
            // Any cleanup can go here (e.g., stop Docker containers, etc.)
            echo "Pipeline completed."
        }

        success {
            // This block runs if the pipeline is successful
            echo "Pipeline was successful!"
        }

        failure {
            // This block runs if the pipeline fails
            echo "Pipeline failed."
        }
    }
}
