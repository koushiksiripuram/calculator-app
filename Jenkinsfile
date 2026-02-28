pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
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

        stage('Install to Local Repo') {
            steps {
                sh 'mvn install'
            }
        }
    }

    post {
        success {
            echo 'Library JAR packaged and installed successfully'
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        failure {
            echo 'Build failed'
        }
    }
}
