pipeline {
    agent any

    environment {
        DOCKER_VERSION = "v1.0" // Puedes cambiar esto por la versión que desees
        DOCKER_REGISTRY = "safernandez666"
        COSIGN_PUBLIC_KEY = credentials('cosign-public-key')
    }

    stages {
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
                    sh "docker push ${DOCKER_REGISTRY}/webserver:latest"
                }
            }
        }
        stage('verify the container image') {
            steps {
                sh 'cosign version'
                sh "cosign verify --key $COSIGN_PUBLIC_KEY ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION}"
            }
        }
    }
}
