pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/shazforiot/nodeapp_test.git'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh "npm run build"
            }
        }

        stage('Build image') {
            steps {
                sh "docker build -t nodeapp:latest ."
            }
        }

        stage('Docker push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    sh "docker tag nodejs:v1 sasikanth777/nodejs:v1"
                    sh "docker push sasikanth777/nodejs:v1"
                    sh "docker logout"
                }
            }
        }

        stage('k8s deploy') {
            steps {
                sh 'kubernetesDeploy(configs: "deployment.yml", kubeconfigId: "kubernetes")'
            }
        }
    }
}
