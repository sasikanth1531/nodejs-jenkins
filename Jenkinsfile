pipeline {
    environment {
        dockerimagename = 'sasikanth/nodeapp'
        registryCredential = 'dockerhublogin'
    }

    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/sasikanth1531/nodejs-jenkins.git'
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build(dockerimagename)
                }
            }
        }

        stage('Pushing Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploying App to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(configs: 'deployment.yml', kubeconfigId: 'kubernetes')
                }
            }
        }
    }
}
