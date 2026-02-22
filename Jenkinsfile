pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/local/go/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                // В pipeline from SCM это обычно не обязательно, но для наглядности:
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh '''
                    go version
                    go test .
                '''
            }
        }

        stage('Build Docker image') {
            steps {
                sh '''
                    docker version
                    docker build .
                '''
            }
        }
    }
}
