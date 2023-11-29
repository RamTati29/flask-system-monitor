pipeline {
    agent any
    
    stages{
        stage('clone code'){
            steps{
                echo "cloning the code"
                git url:"https://github.com/RamTati29/django-notes-app.git", branch:"main"
            }
        }
        stage('build'){
            steps{
                echo "building code"
                sh "docker build -t my-note-app ."
            }
        }
        stage('push to dockerhub'){
            steps{
                echo "pushing the image to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker tag my-note-app ${env.dockerhubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker push ${env.dockerhubUser}/my-note-app:latest"
                }
            }
        }
        stage('deploy'){
            steps{
                echo "deploying to container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}