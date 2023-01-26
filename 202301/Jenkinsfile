pipeline {
    agent any

    stages {
        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("nicolasmatos/kube-news:202301.${env.BUILD_ID}", '-f ./src/dockerfile ./src')
                }
            }
        }

        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("202301.${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Deploy Kubernetes') {
            environment {
                tag_version = "202301.${env.BUILD_ID}"
            }
            steps {
                script {
                    withKubeConfig ([credentialsId: 'kubeconfig']) {
                        sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                        sh 'kubectl apply -f ./k8s/deployment.yaml'
                    }
                }
            }
        }
    }
}
