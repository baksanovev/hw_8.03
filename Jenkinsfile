pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/local/go/bin"
        NEXUS_URL  = "http://10.0.2.15:8081"          // адрес твоего Nexus
        NEXUS_REPO = "go-binaries"                    // имя raw-hosted репо
    }

    stages {
        stage('Checkout') {
            steps {
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

        stage('Build binary') {
            steps {
                sh '''
                    # собираем бинарник, имя можешь выбрать своё
                    go build -o app-linux-amd64 .
                '''
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-creds',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh '''
                        curl -v -u "$NEXUS_USER:$NEXUS_PASS" \
                          --upload-file app-linux-amd64 \
                          "$NEXUS_URL/repository/$NEXUS_REPO/app-linux-amd64"
                    '''
                }
            }
        }
    }
}
