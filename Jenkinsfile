pipeline {
    agent any

    environment {
        DOCKER_USER    = "arunima10pooja"
        IMAGE_NAME     = "java-hello-world"
        BUILD_TAG      = "${BUILD_ID}"
        FULL_IMAGE     = "${DOCKER_USER}/${IMAGE_NAME}:${BUILD_TAG}"
        CONTAINER_NAME = "java-test-container"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/arunima-pooja/jenkins_java1.git'
            }
        }

        stage('Build & Local Tag') {
            steps {
                sh "docker build -t ${FULL_IMAGE} ."
                sh "docker tag ${FULL_IMAGE} ${DOCKER_USER}/${IMAGE_NAME}:latest"
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'arunima_cred', 
                                                 usernameVariable: 'USERNAME', 
                                                 passwordVariable: 'PASSWORD')]) {
                    sh "echo \$PASSWORD | docker login -u \$USERNAME --password-stdin"
                    sh "docker push ${FULL_IMAGE}"
                    sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Verify Registry') {
            steps {
                sh "docker manifest inspect ${FULL_IMAGE}"
                echo "Image verified in DockerHub!"
            }
        }
    }

    post {
        always {
            script {
                sh "docker logout"
                sh "docker rm -f ${CONTAINER_NAME}"
                sh "docker rmi -f ${FULL_IMAGE} ${DOCKER_USER}/${IMAGE_NAME}:latest"
            }
        }
    }
}
