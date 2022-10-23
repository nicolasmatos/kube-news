pipeline {
    agent any
    
    stages {

        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("nicolasmatos/kube-news:${env.BUILD_ID}", '-f ./src/dockerfile ./src')
                }
            }
        }

    }
}