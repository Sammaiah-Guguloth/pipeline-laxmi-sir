pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sammaiah1919/my-k8s-app"
        KUBECONFIG = "/home/user/.kube/config"
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Sammaiah-Guguloth/pipeline-laxmi-sir.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t my-k8s-app:${BUILD_NUMBER} .

                docker tag my-k8s-app:${BUILD_NUMBER} $DOCKER_IMAGE:latest
                docker tag my-k8s-app:${BUILD_NUMBER} $DOCKER_IMAGE:${BUILD_NUMBER}
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker push $DOCKER_IMAGE:latest
                docker push $DOCKER_IMAGE:${BUILD_NUMBER}
                '''
            }
        }

        stage('Verify Kubernetes Cluster') {
            steps {
                sh '''
                kubectl --kubeconfig=$KUBECONFIG get nodes
                '''
            }
        }

        

        stage('Load image to kubernates') {
            steps {
                sh '''
                    minikube image load sammaiah1919/my-k8s-app:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl --kubeconfig=$KUBECONFIG apply -f k8s/deployment.yaml
                kubectl --kubeconfig=$KUBECONFIG apply -f k8s/service.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                minikube service my-k8s-app-service
                '''
            }
        }
    }
}