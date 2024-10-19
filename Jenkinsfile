pipeline {
    agent any

    // Definir os stages
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("rodrigoafonso/guia-pratico-jenkins:v.${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("v.${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy Docker Image') {
            environment {
                tag_version = "v.${env.BUILD_ID}"
            }
            steps {
                withKubeConfig([credentionsID: 'kubeconfig'])
                sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/deployment.yaml'
                sh 'kubectl apply -f ./k8s/deployment.yaml'
            }
        }
    }

}