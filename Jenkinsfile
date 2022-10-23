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

        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Deploy Kubernetes') {
            steps {
                script {
                    withKubeConfig ([credentialsId: 'kubeconfig') {
                        sh 'kubectl apply -f ./k8s/deployment.yaml'
                    }
                }
            }
        }
    }
}