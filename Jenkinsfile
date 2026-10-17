
pipeline {
    agent any

    environment {
        MINIKUBE_HOME = 'C:\\Users\\jaiwa'
        KUBECONFIG = 'C:\\Users\\jaiwa\\.kube\\config'
    }

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
                bat 'set "PATH=C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;%PATH%" && "C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t exp6-html:v1 .'
            }
        }

        stage('Check Minikube') {
            steps {
                bat 'set "PATH=C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;%PATH%" && "C:\\Program Files\\Kubernetes\\Minikube\\minikube.exe" status --profile=minikube'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                bat 'set "PATH=C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;%PATH%" && "C:\\Program Files\\Kubernetes\\Minikube\\minikube.exe" image load exp6-html:v1 --profile=minikube'
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                bat '"C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" apply -f deployment.yaml'
                bat '"C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" apply -f service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '"C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" get deployments'
                bat '"C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" get pods'
                bat '"C:\\Users\\jaiwa\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" get services'
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
