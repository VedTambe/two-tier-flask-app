pipeline {
    agent any

    stages {
        stage("Code Clone") {
            steps {
                git url: "https://github.com/VedTambe/two-tier-flask-app.git", branch: "master"
            }
        }

        stage("Code Build") {
            steps {
                sh "docker build -t ved-app ."
            }
        }

        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag ved-app ${env.dockerHubUser}/ved-app"
                sh "docker push ${env.dockerHubUser}/ved-app:latest"
            
                }    
            }
        }

        stage("Code Deploy") {
            steps {
                sh "docker compose up"
            }
        }
    }
}
