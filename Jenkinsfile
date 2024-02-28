pipeline {
    agent any

    environment {
        DOCKER_VERSION = "v1.0" // Puedes cambiar esto por la versión que desees
        DOCKER_REGISTRY = "safernandez666"
    }

    stages {
        stage('cleanup') {
            steps {
                sh 'docker system prune -a --volumes --force'
            }
        }
        stage('docker build') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION} -t ${DOCKER_REGISTRY}/webserver:latest -f Dockerfile ."
                }
            }
        }
        stage('docker push') {
            steps {
                script {
                    sh "docker push ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION}"
                }
            }
        }
        stage('sign the container image') {
            steps {
                withCredentials([file(credentialsId: 'cosign-public-key', variable: 'COSIGN_PUBLIC_KEY_FILE')]) {
                    sh 'cosign version'
                    sh "cosign sign --key ${COSIGN_PUBLIC_KEY_FILE} ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION}"
                }
            }
        }
    }
}