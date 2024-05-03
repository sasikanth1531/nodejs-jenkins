pipeline {

    environment {
        dockerimagename = 'sasikanth/nodeapp'
        dockerImage = "node"
    }

    agent any

    stages {

        stage('Checkout Source') {
            steps {
                git 'https://github.com/sasikanth1531/nodejs-jenkins.git'
            }
        }

        stage('Build image') {
            steps{
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }

        stage('Pushing Image') {
            environment {
                registryCredential = 'dockerhublogin'
            }
            steps{
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying App to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(configs: "deployment.yml", kubeconfigId: "kubernetes")
                }
            }
        }
    }
}
