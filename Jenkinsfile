pipeline {

    agent any

    environment {
        DOCKER = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'
        KUBECTL = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe'
    }

    stages {

        stage('Clone Code') {
            steps {

                echo 'Cloning College Placement Portal from GitHub'

                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {

                echo 'Building Docker image'

                bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" build -t balaji00002/college-placement-portal:v1 .'
            }
        }

        stage('Test Application') {
            steps {

                echo 'Testing Docker image'

                bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" images balaji00002/college-placement-portal:v1'
            }
        }

        stage('Push Image') {
            steps {

                echo 'Pushing image to Docker Hub'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'

                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" push balaji00002/college-placement-portal:v1'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {

                echo 'Deploying Placement Portal to Kubernetes'

                withCredentials([
                    file(
                        credentialsId: 'kubeconfig',
                        variable: 'KUBECONFIG'
                    )
                ]) {

                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe" apply -f deployment.yaml'
                }
            }
        }

        stage('Verify Kubernetes') {
            steps {

                echo 'Checking Deployment'

                withCredentials([
                    file(
                        credentialsId: 'kubeconfig',
                        variable: 'KUBECONFIG'
                    )
                ]) {

                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe" get deployment college-placement-portal'

                    echo 'Checking 3 Kubernetes Pods'

                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe" get pods'

                    echo 'Checking NodePort Service'

                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe" get service college-placement-portal-service'
                }
            }
        }
    }

    post {

        success {
            echo 'College Placement Portal CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}