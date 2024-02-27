pipeline {
    agent any

    stages {
        stage('docker build') {
            steps {
                script {
                    sh "docker build -t safernandez666/webserver:${BUILD_ID} -f Dockerfile ."
                }
            }
        }
        stage('docker push') {
            steps {
                script {
                    sh "docker push safernandez666/webserver:${BUILD_ID}"
                }
            }
        }
    }
}