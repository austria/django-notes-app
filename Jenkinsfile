pipeline {
    agent any
    
    stages {
        stage("code") {
            steps {
                echo "cloning the code"
                git url: "https://github.com/austria/django-notes-app.git", branch: "main"
            }
        }
        stage("build") {
            steps {
                echo "building the code"
                sh "docker build -t mynotes-app . --no-cache"
            }
        }
        stage("push to docker hub") {
            steps {
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    sh 'docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD'
                    // Tag the local image
                sh "docker tag mynotes-app:latest mehranabbasi/myfirstimagepush"
                
                // Push the image to Docker Hub
                sh "docker push mehranabbasi/myfirstimagepush"
                   
                }
            }
        }
        stage("deploy") {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
               
            }
        }
    }
}
