pipeline {
    agent any

    environment {
        IMAGE_NAME = "raj200518/my-app"
    }

    stages {

        stage('Clone Source') {
            steps {
                git branch: 'main', url: 'https://github.com/Raj-2005-cloud/my-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Windows Jenkins
                bat 'docker build -t %IMAGE_NAME%:latest .'

                // If Linux/WSL, use:
                // sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_TOKEN')]) {

                    // Windows
                    bat 'echo %DOCKER_TOKEN% | docker login -u raj200518 --password-stdin'

                    // Linux alternative:
                    // sh 'echo $DOCKER_TOKEN | docker login -u raj200518 --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Windows
                bat 'docker push %IMAGE_NAME%:latest'

                // Linux alternative:
                // sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                // Stop old container (ignore error if not exists)
                bat 'docker stop my-app-container || exit 0'
                bat 'docker rm my-app-container || exit 0'

                // Run new container
                bat 'docker run -d -p 5000:5000 --name my-app-container %IMAGE_NAME%:latest'
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD Pipeline Completed Successfully!'
        }
        failure {
            echo '❌ Pipeline Failed! Check logs.'
        }
    }
}
