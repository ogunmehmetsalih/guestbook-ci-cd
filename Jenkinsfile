pipeline {
    agent any

    environment {
        KUBECONFIG = "${HOME}/.kube/config"
    }

    stages {
        stage('Deploy Redis Leader') {
            steps {
                sh 'kubectl apply -f redis-leader-deployment.yaml'
                sh 'kubectl apply -f redis-leader-service.yaml'
            }
        }
        stage('Deploy Redis Followers') {
            steps {
                sh 'kubectl apply -f redis-follower-deployment.yaml'
                sh 'kubectl apply -f redis-follower-service.yaml'
            }
        }
        stage('Deploy Frontend') {
            steps {
                sh 'kubectl apply -f frontend-deployment.yaml'
                sh 'kubectl apply -f frontend-service.yaml'
            }
        }
    }
}

