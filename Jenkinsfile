pipeline {
    agent any
    tools {
        jdk 'jdk'
        nodejs 'nodejs'
        dockerTool 'docker' // Utilisation de l'outil Docker configuré dans Jenkins
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
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                }
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                    // Utiliser la commande Docker sans sudo pour construire l'image
                    docker.build('netflix')
                }
            }
        }
        stage('Tag Docker image') {
            steps {
                script {
                    // Utiliser la commande Docker sans sudo pour taguer l'image
                    docker.image('netflix').tag("bilelfarhat/netflix:latest")
                }
            }
        }
        stage('Push Docker image to Google Container Registry') {
            steps {
                script {
                    // Utiliser la commande Docker sans sudo pour pousser l'image
                    docker.withRegistry('https://gcr.io', 'gcr-credentials') {
                        docker.image("bilelfarhat/netflix:latest").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    dir('Kubernetes') {
                        withKubeConfig(
                            caCertificate: '',
                            clusterName: 'your-cluster-name',
                            contextName: 'your-context-name',
                            credentialsId: 'your-kubernetes-credentials-id',
                            namespace: 'your-namespace',
                            restrictKubeConfigAccess: false,
                            serverUrl: 'https://your-kubernetes-api-url'
                        ) {
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
