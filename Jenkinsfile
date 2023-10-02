pipeline {
    agent any
    stages {
        stage("code") {
            steps {
                echo "code cloned"
                git url: "https://github.com/Pardeep32/django-notes-app.git", branch: "main"
            }
        }
        stage("build") {
            steps {
                echo "building the image"
                sh "docker build . -t my-note-app"
            }
        }
        stage("push to docker hub") {
            steps {
                echo "pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "docker-hubid", passwordVariable: "dockerHubPass", usernameVariable: "dockerhubuser")]) {
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u \$dockerhubuser -p \$dockerHubPass"
                sh "docker push ${env.dockerHubUser}/my-note-app"    
                    
                }
            }
        }
        stage("deploy") {
            steps {
                echo "deploy the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
