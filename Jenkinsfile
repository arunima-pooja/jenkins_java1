pipeline {
    agent any

    environment {
        // Define the image name
        DOCKER_IMAGE = "java-hello-world:${env.BUILD_ID}"
        CONTAINER_NAME = "java-app-container-${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/arunima-pooja/jenkins_java1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Run Container') {
            steps {
                sh "docker run --name ${CONTAINER_NAME} ${DOCKER_IMAGE}"
            }
        }

        stage('Verify & Cleanup') {
            steps {
                // View logs to verify output and then remove the container
                sh "docker logs ${CONTAINER_NAME}"
                sh "docker rm ${CONTAINER_NAME}"
            }
        }
    }
    
    post {
        always {
            // Clean up the local image to save space
            sh "docker rmi ${DOCKER_IMAGE}"
        }
    }
}
