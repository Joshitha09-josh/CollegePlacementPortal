pipeline {

    agent any

    environment {

        DOCKER = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'

        KUBECTL = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe'

    }


    stages {


        stage('Clone Code') {

            steps {

                echo 'Cloning updated College Placement Portal from GitHub'

                checkout scm

            }

        }


        stage('Build Docker Image') {

            steps {

                echo 'Building College Placement Portal Docker Image v2'

                bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" build -t balaji00002/college-placement-portal:v2 .'

            }

        }


        stage('Test Application') {

            steps {

                echo 'Testing Docker Image v2'

                bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" images balaji00002/college-placement-portal:v2'

            }

        }


        stage('Push Image') {

            steps {

                echo 'Authenticating with Docker Hub and pushing Image v2'

                withCredentials([

                    usernamePassword(

                        credentialsId: 'dockerhub-credentials',

                        usernameVariable: 'DOCKER_USERNAME',

                        passwordVariable: 'DOCKER_PASSWORD'

                    )

                ]) {


                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'


                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" push balaji00002/college-placement-portal:v2'

                }

            }

        }


        stage('Deploy to Kubernetes') {

            steps {

                echo 'Deploying College Placement Portal v2 to Kubernetes'


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

                echo 'Checking Kubernetes Deployment'


                withCredentials([

                    file(

                        credentialsId: 'kubeconfig',

                        variable: 'KUBECONFIG'

                    )

                ]) {


                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe" get deployment college-placement-portal'


                    echo 'Checking three Kubernetes Pods'


                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe" get pods'


                    echo 'Checking NodePort Service'


                    bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe" get service college-placement-portal-service'

                }

            }

        }

    }


    post {

        success {

            echo 'College Placement Portal v2 CI/CD Pipeline completed successfully!'

        }


        failure {

            echo 'Pipeline failed. Check the console output.'

        }

    }

}