pipeline {
    agent any

    // Definir os stages
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("rodrigowafonso/guia-pratico-jenkins:${env.BUILD.ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKER_HUB') {
                        dockerapp.push('latest')
                        dockerapp.push("v.${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy Docker Image') {
            steps {
                sh 'echo "Executando o comando Kubectl Apply"'
            }
        }
    }

}