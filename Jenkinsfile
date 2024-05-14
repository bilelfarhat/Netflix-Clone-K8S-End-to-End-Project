pipeline {
    agent any
    tools {
        // Define tools to be installed by Jenkins
        jdk 'jdk'
        nodejs 'nodejs'
        docker 'docker' // Specify Docker as a tool to be installed
    }
    stages {
        stage('Checkout repository') {
            steps {
                git branch: 'master', url: 'https://github.com/bilelfarhat/DeepLearning-Fruit_Vegetable_Prediction.git'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                }
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'docker build -t netflix .' // Use Docker tool to build the image
            }
        }
        stage('Tag Docker image') {
            steps {
                sh 'docker tag netflix bilelfarhat/netflix:latest' // Use Docker tool to tag the image
            }
        }
        stage('Push Docker image to Google Container Registry') {
            steps {
                sh 'docker push bilelfarhat/netflix:latest' // Use Docker tool to push the image
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    dir('Kubernetes') {
                        withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: '', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                            sh 'kubectl apply -f deployment.yml'
                            sh 'kubectl apply -f service.yml'
                            sh 'kubectl get svc'
                            sh 'kubectl get all'
                        }
                    }
                }
            }
        }
    }
}
