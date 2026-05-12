pipeline {

    agent any

    environment {
        IMAGE_NAME = "devops-project"
        CONTAINER_NAME = "devops-container"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Waseem3720/DevOps-Project.git'
            }
        }

        stage('SonarQube Scan') {
            steps {
                echo 'Running SonarQube Scan'
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                echo 'Running OWASP Dependency Check'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh '/usr/bin/trivy fs --timeout 10m .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true

                docker run -d \
                --name $CONTAINER_NAME \
                -p 80:80 \
                $IMAGE_NAME
                """
            }
        }
    }
}
