pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                echo 'Cloning the code ...'
                git url:'https://github.com/shweta1097/django-notes-app.git', branch:'main'
            }
        }
        stage('Build') {
             steps {
                 echo 'Building the code ...'
                 sh 'docker build -t my-notes-app .'
             }
        }
        stage('Push to docker hub') {
             steps {
                 echo 'Pusing the image to docker hub ...'
                 withCredentials([usernamePassword(credentialsId:'DOCKER_HUB',passwordVariable:'dockerhubpass',usernameVariable:'dockerhubusername')]){
                 sh "docker tag my-notes-app ${env.dockerhubusername}/my-notes-app:latest "     
                 sh "docker login -u ${env.dockerhubusername} -p ${env.dockerhubpass}"
                 sh "docker push ${env.dockerhubusername}/my-notes-app:latest "
                 }
             }
        }
        stage('Deploy'){
            steps {
                echo 'Deploying the container ...'
                sh "docker-compose down && docker-compose up -d"
            }
        }
            
    }
 }
