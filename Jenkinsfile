pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "springboot-demo"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop old container if exists
                    sh """
                        docker ps -q --filter "name=springboot-app" | grep -q . && docker stop springboot-app || true
                        docker rm springboot-app || true
                    """
                    // Run new container
                    sh "docker run -d --name springboot-app -p 8080:8080 ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Application deployed successfully! Access it at http://<Jenkins-Server-IP>:8080"
        }
        failure {
            echo "❌ Build or deployment failed."
        }
    }
}
