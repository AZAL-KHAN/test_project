pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "flask-jenkins-app"     // Local Docker image
        CONTAINER_NAME = "flask-container"     // Local test container
    }

    stages {
        stage("Checkout Code") {
            steps {
                echo "Pulling code from GitHub..."
                git branch: 'main', url: 'https://github.com/AZAL-KHAN/test_project.git'
            }
        }

        stage("Build Docker Image") {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage("Run & Test Container") {
            steps {
                echo "Running container for testing..."
                // Stop old container if exists
                sh 'docker stop $CONTAINER_NAME || true && docker rm $CONTAINER_NAME || true'
                // Run new container
                sh 'docker run -d -p 5000:5000 --name $CONTAINER_NAME $DOCKER_IMAGE:latest'

                echo "Waiting for container to start..."
                sh 'sleep 5'

                echo "Checking if container is running..."
                sh 'docker ps | grep $CONTAINER_NAME'

                echo "Testing Flask app endpoint..."
                sh 'curl -f http://localhost:5000 || (echo "App is not responding!" && exit 1)'
            }
        }

        stage("Stop Test Container") {
            steps {
                echo "Stopping test container..."
                sh 'docker stop $CONTAINER_NAME'
                sh 'docker rm $CONTAINER_NAME'
            }
        }

        stage("Deploy to Kubernetes") {
            steps {
                echo "Deploying app to Kubernetes..."
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'

                echo "Checking deployed pods..."
                sh 'kubectl get pods -l app=flask-app'
            }
        }
    }
}
