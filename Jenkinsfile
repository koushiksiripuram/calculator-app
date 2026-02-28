pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    environment {
        DOCKER_IMAGE = "koushiksiripuram/calculator-app:1.0"
    }

    stages {

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Package JAR') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }
        stage('Deploy to Kubernetes') {
    steps {
        sh 'kubectl apply -f k8s-deployment.yaml'
        sh 'kubectl get pods'
    }
}
    }
}
