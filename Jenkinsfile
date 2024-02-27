pipeline {
    agent any

    environment {
        DOCKER_VERSION = "v1.0" // Puedes cambiar esto por la versión que desees
        DOCKER_REGISTRY = "safernandez666"
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
        stage('cosing') {
            steps {
                script {
                    // Obtener el digest de la imagen desde Docker Hub
                    sh 'digest=$(docker inspect --format="{{index .RepoDigests 0}}" ${DOCKER_REGISTRY}/webserver:latest)'
                    // Extraer el digest de la cadena devuelta
                    sh 'cosign sign --key /keys/cosign.key -y "${DOCKER_REGISTRY}/webserver:latest@${digest}"'
                }
            }
        }
    }
}