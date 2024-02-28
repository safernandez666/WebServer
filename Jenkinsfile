pipeline {
    agent any

    environment {
        DOCKER_VERSION = "v1.0" // Puedes cambiar esto por la versión que desees
        DOCKER_REGISTRY = "safernandez666"
        COSIGN_PASSWORD=credentials('cosign-password')
        COSIGN_PRIVATE_KEY=credentials('cosign-private-key')
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
                    sh 'docker build -t ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION} -t ${DOCKER_REGISTRY}/webserver:latest -f Dockerfile .'
                    sh "docker tag ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION} ${DOCKER_REGISTRY}/webserver:latest"
                }
            }
        }
        stage('docker push') {
            steps {
                script {
                    sh 'docker push ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION}'
                    sh "docker push ${DOCKER_REGISTRY}/webserver:latest"
                }
            }
        }
        stage('sign the container image') {
            steps { // Firmamos la Imagen
                withCredentials([file(credentialsId: 'cosign-private-key', variable: 'COSIGN_PRIVATE_KEY_FILE')]) {
                    sh 'cosign version'
                    sh 'cosign sign --key ${COSIGN_PRIVATE_KEY_FILE} -y ${DOCKER_REGISTRY}/webserver:${DOCKER_VERSION}'
                }
            }
        }
    }
}
