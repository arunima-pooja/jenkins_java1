pipeline {
    agent any

    environment {
        DOCKER_IMAGE   = "java-hello-world:${env.BUILD_ID}"
        CONTAINER_NAME = "java-app-container-${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/arunima-pooja/jenkins_java1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build --pull -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Run & Verify') {
            steps {
                sh "docker run --name ${CONTAINER_NAME} ${DOCKER_IMAGE}"
                sh "docker logs ${CONTAINER_NAME}"
            }
        }
    }

    post {
        always {
	    sh "docker rm -f ${CONTAINER_NAME}"
            sh "docker rmi ${DOCKER_IMAGE}"
        }
    }
}
