pipeline {
    agent any

    tools {
        maven 'Maven_3.9.4' // Match the Maven name in Jenkins tool config
        jdk 'JDK_17'        // Match the JDK name in Jenkins tool config
    }

    environment {
        DOCKER_IMAGE = "ajayjoshi119418/maven-docker-build:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Uses Jenkins automatic SCM checkout
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker run -d -p 8080:8080 ${DOCKER_IMAGE}"
            }
        }
    }

    post {
        failure {
            echo '❌ Build or deployment failed.'
        }
        success {
            echo '✅ Build and deployment successful!'
        }
    }
}
