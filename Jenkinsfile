pipeline {

    agent any

    environment {
        IMAGE_NAME = "devops-project"
        CONTAINER_NAME = "devops-container"
        TRIVY_CACHE_DIR = "/var/lib/jenkins/trivy-cache"
        EC2_HOST = "52.205.233.81"
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
                echo 'SonarQube Scan skipped (not configured yet)'
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                echo 'OWASP Dependency Check skipped (not configured yet)'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh '''
                export TRIVY_CACHE_DIR=/var/lib/jenkins/trivy-cache
                mkdir -p $TRIVY_CACHE_DIR

                /usr/bin/trivy image \
                    --scanners vuln \
                    --cache-dir $TRIVY_CACHE_DIR \
                    --timeout 10m \
                    $IMAGE_NAME
                '''
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@$EC2_HOST << 'EOF'

                echo "Stopping old container..."
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true

                echo "Running new container..."
                docker run -d \
                    --name $CONTAINER_NAME \
                    -p 80:80 \
                    $IMAGE_NAME

                echo "Deployment successful!"

                EOF
                '''
            }
        }
    }
}
