pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("sanmathisedhupathi/recipenest:latest")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    sh """
                    docker login -u sanmathisedhupathi -p 08-Sep-2004
                    docker push sanmathisedhupathi/recipenest:latest
                    """
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'minikube delete'
                    sh 'minikube start'
                    sh 'helm repo add prometheus-community https://prometheus-community.github.io/helm-charts'
                    sh 'helm repo update'
                    sh 'helm install prometheus prometheus-community/kube-prometheus-stack'
                    sh 'kubectl get pods -n default'
                    sh 'kubectl apply -f kubernetes/deployment.yaml'
                    sh 'kubectl apply -f kubernetes/service.yaml'
                    sh 'kubectl apply -f kubernetes/ingress.yaml'
                    sh 'kubectl apply -f service-monitor.yaml'
                }
            }
        }
    }
}