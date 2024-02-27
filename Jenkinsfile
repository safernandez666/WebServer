pipeline {
    agent any

    stages {
        stage('docker build') {
            steps {
                script {
                    sh "docker build -f Dockerfile -t safernandez666/webserver:${BUILD_ID}"
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