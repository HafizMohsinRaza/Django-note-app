pipeline {
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/HafizMohsinRaza/Django-note-app.git", branch: "main"
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Push the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: 'dockerhubPass', usernameVariable: 'dockerhubUser')]) {
                    sh "docker login -u \$dockerhubUser -p \$dockerhubPass"
                    sh "docker push ${env.dockerhubUser}/notes-app:latest"

                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploy the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    post {
        always {
            // Clean up Docker credentials
            sh "docker logout"
        }
    }
}
