pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/shazforiot/nodeapp_test.git'
            }
        }
        
        stage('Docker build and push') {
            steps {
                sh "docker build -t nodeapp:latest ."
                
                withCredentials([usernamePassword(credentialsId: 'docker_cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    sh "docker tag nodeapp:latest sasikanth777/nodejs:v1"
                    sh "docker push sasikanth777/nodejs:v1"
                }
            }
        }

        stage('Kubernetes deploy') {
            steps {
                sh 'kubernetesDeploy(configs: "deployment.yml", kubeconfigId: "kubernetes")'
            }
        }
    }
}
