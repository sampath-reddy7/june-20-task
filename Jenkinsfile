pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username
        DOCKER_HUB_USERNAME = 'sampathreddy926'
        // Replace with your repository name on Docker Hub (e.g., my-website-image)
        // It's common to use your Docker Hub username as a prefix: your-dockerhub-username/my-website
        DOCKER_IMAGE_NAME = "${sampathreddy926}/my-website"
        // The ID of the Jenkins credential you created for Docker Hub
        DOCKER_HUB_CREDENTIAL_ID = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                //git 'https://github.com/sampath-reddy7/june-20-task.git'
                 echo "Checking out code from GitHub..."
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${my-website-barista}:${env.BUILD_NUMBER}")
                    echo "Docker image built: ${dockerImage.id}"
                }
            }
        }

        stage('Run Container (Local Test)') {
            steps {
                script {
                    // This step is optional but highly recommended for testing before pushing
                    // It runs the container locally on the Jenkins agent
                    docker.image("${my-website-barista}:${env.BUILD_NUMBER}").withRun('-p 8097:80') { c ->
                        echo "Container running on port 8097. Sleeping for 10 seconds for verification..."
                        sleep 10
                        // You could add a curl command here to check if the website is accessible
                        // For example: sh 'curl -f http://localhost:8081 || exit 1'
                        echo "Container test complete."
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Using withCredentials to securely access Docker Hub credentials
                    withCredentials([usernamePassword(credentialsId: dockerhub-credentials, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "docker login -u ${sampathreddy926} -p ${Dineshreddy123\\@}"
                        dockerImage.push()
                        dockerImage.push('latest') // Also tag as 'latest' for convenience
                        sh "docker logout"
                    }
                }
            }
        }
    }
    
}