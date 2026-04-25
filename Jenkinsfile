pipeline {
    agent any
    environment {
        DOCKER_USER  = "arunima10pooja"
        IMAGE_NAME   = "java-hello-world"
        DOCKER_IMAGE = "${DOCKER_USER}/${IMAGE_NAME}:${BUILD_ID}"
    }
    stages {
        stage('Build & Push') {
            steps {
                git branch: 'main', url: 'https://github.com/arunima-pooja/jenkins_java1.git'
                sh "docker build -t ${DOCKER_IMAGE} ."
                withCredentials([usernamePassword(credentialsId: 'arunima_cred', usernameVariable: 'USER', passwordVariable: 'PASSWORD')]) {
                    sh "echo \$PASSWORD | docker login -u \$USER --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
        stage('K8s Deploy') {
            steps {
                    sh "kubectl apply -f deployment.yaml"
                    sh "kubectl rollout status deployment/java-hello-world"
                }
	    }	
        }
	stage('Verify Deployment') {
	   steps {
		    sh "kubectl get deployment"
		    sh "kubectl get pods"
        }
    }
    post {
        always {
            sh "docker rmi -f ${DOCKER_IMAGE}"
            sh "docker logout"
        }
    }
}
