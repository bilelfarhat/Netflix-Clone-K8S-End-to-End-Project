pipeline {
    agent any
    tools {
        jdk 'jdk'
        nodejs 'nodejs'
        dockerTool 'docker'
    }
    stages {
        stage('Checkout repository') {
            steps {
                // Use 'git' command to checkout the repository
                git branch: 'master', url: 'https://github.com/bilelfarhat/DeepLearning-Fruit_Vegetable_Prediction.git'
            }
        }
        stage('run Docker') {
            steps {
                sh 'systemctl start docker'
                sh 'systemctl enable docker'
                sh 'usermod -aG docker jenkins'
            }
        }    
        stage('Login to Docker Hub') {
            steps {
                // Use Docker Hub credentials
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                }
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'sudo docker build -t netflix .'
            }
        }

        stage('Tag Docker image') {
            steps {
                sh 'sudo docker tag netflix bilelfarhat/netflix:latest'
            }
        }

        stage('Push Docker image to Google Container Registry') {
            steps {
                sh 'sudo docker push bilelfarhat/netflix:latest'
            }
        }

      //  stage('SonarQube analysis') {
            //steps {
                // Run SonarQube analysis
                //sh 'sonar-scanner'
            //}
        //}

        stage('Deploy to Kubernetes'){
            steps{
                script{
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