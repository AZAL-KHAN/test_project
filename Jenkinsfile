pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "flask-jenkins-app"
    }

   stages {
    stage ("Checkout code") {
        steps {
            git branch: "main" , url: "https://github.com/AZAL-KHAN/test_project.git"
        }
    }
    stage ("Build the docker image") {
        steps {
            echo "Building the docker image...."
            sh "docker build -t ${DOCKER_IMAGE}:latest ."
        }
    }

    stage ("Run the container") {
        steps {
            sh "docker run -d -p 5000:5000 --name flask_container $DOCKER_IMAGE:latest"
        }
    }
   }
}