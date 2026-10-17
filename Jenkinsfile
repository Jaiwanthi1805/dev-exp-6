pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                bat 'if not exist index.html exit /b 1'
                echo 'HTML file exists - Test passed'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t exp6-html:v1 .'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                bat 'minikube image load exp6-html:v1'
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'kubectl get deployments'
                bat 'kubectl get pods'
                bat 'kubectl get services'
            }
        }
    }

    post {
        success {
            echo 'DevOps CI/CD pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}
