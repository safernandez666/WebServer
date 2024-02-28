pipeline {
    agent any

    environment {
        DOCKER_VERSION = "v1.0" // Puedes cambiar esto por la versión que desees
        DOCKER_REGISTRY = "safernandez666"
        COSIGN_PUBLIC_KEY = credentials('cosign-public-key')
        COSIGN_PRIVATE_KEY = credentials('cosign-private-key')
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
                sh 'cosign version'
                sh "cat $COSIGN_PUBLIC_KEY'"
                sh "cosign sign --key $COSIGN_PUBLIC_KEY ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION}"
            }
        }
    }
}
