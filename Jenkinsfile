pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "myimg:${env.BUILD_ID}" // Versioning the Docker image
    }
    stages {
        stage('Checkout Code from GitHub') {
            steps {
                git url: 'https://github.com/Nithishprabulingam/Banking-java-project'
                echo 'Checked out code from GitHub'
            }
        }
        stage('Compile Code') {
            steps {
                echo 'Starting compilation'
                sh 'mvn compile'
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'mvn test' // Remove || true to fail the build on test failures
            }
        }
        stage('QA Check') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Package Application') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }
        stage('Expose Port') {
            steps {
                sh "docker run -dt -p 8091:8091 --name c000 ${DOCKER_IMAGE}"
            }
        }
        stage('Cleanup') {
            steps {
                echo 'Cleaning up Docker images and containers'
                sh 'docker rm -f c000 || true' // Remove the container
                sh "docker rmi ${DOCKER_IMAGE} || true" // Remove the image
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished!'
            // Optional: Add notification step here
        }
        failure {
            echo 'Build failed! Notifying team...'
            // Optional: Add failure notification step here
        }
    }
}
