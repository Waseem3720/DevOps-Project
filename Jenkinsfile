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
        --download-db-only \
        --cache-dir $TRIVY_CACHE_DIR \
        --timeout 15m || true

        /usr/bin/trivy image \
        --skip-db-update \
        --scanners vuln \
        --cache-dir $TRIVY_CACHE_DIR \
        --timeout 15m \
        $IMAGE_NAME
        '''
    }
}
        stage('Deploy to EC2') {
    steps {
        sh """
        ssh -o StrictHostKeyChecking=no ubuntu@52.205.233.81 << EOF

        docker stop devops-container || true
        docker rm devops-container || true

        docker run -d \
        --name devops-container \
        -p 80:80 \
        devops-project

        EOF
        """
    }
}
    }
}
