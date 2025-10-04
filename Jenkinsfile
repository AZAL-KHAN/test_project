pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "flask-jenkins-app"
        CONTAINER_NAME = "flask-container"
    }

    stages {
        stage("Checkout Code") {
            steps {
                git branch: 'main', url: 'https://github.com/AZAL-KHAN/test_project.git'
            }
        }

        stage("Build Docker Image") {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage("Run Container") {
            steps {
                echo "Running container..."
                // Stop old container if exists
                sh 'docker stop $CONTAINER_NAME || true && docker rm $CONTAINER_NAME || true'
                // Run new container
                sh 'docker run -d -p 5000:5000 --name $CONTAINER_NAME $DOCKER_IMAGE:latest'
            }
        }

        stage("Test Container") {
            steps {
                echo "Testing container..."
                // Wait a few seconds for container to start
                sh 'sleep 5'
                // Check if container is running
                sh 'docker ps | grep $CONTAINER_NAME'
                // Test app endpoint (health check)
                sh 'curl -f http://localhost:5000 || (echo "App is not responding!" && exit 1)'
            }
        }

        stage("Stop Container") {
            steps {
                echo "Stopping container..."
                sh 'docker stop $CONTAINER_NAME'
                sh 'docker rm $CONTAINER_NAME'
            }
        }
    }
}
