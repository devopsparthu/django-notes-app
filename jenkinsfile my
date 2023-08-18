pipeline {
    agent any
    
    stages {
        stage('cloned') {
            steps{
                git url:'https://github.com/devopsparthu/django-notes-app.git', branch: 'main'
            }
        }
        stage('build and test') {
            steps{
                sh 'docker build . -t notes-app'
            }
        }
        stage('Login and Image') {
            steps{
                echo "login in to docker hub and pushing image.."
                withCredentials([usernamePassword(credentialsId:'dockerHub',passwordVariable:'dockerHubPassword',usernameVariable:'dockerHubUser')]) {
                    sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage('deploy') {
            steps{
                echo "depolyed.."
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
