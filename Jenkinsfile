@Library("Shared") _
pipeline{
    agent any;
    Stages{
    stage("code"){
        steps{
            git url: "https://github.com/Bahadur143/two-tier-flask-app-demo.git", branch:"dev"
        }
    }
    stage("Build"){
        steps{
            sh "docker build -t two-tier-flask-app ."
        }
    }
    stage("Test"){
        steps{
            echo " Test code"
        }
    }
    stage("Push to docker Hub"){
        steps{
            withCredentials([usernamePassword(credentialsId:"dockerHubCreds",
            passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]) {
                sh "docker login -u ${dockerHubUser} -p {dockerHubPass}"
                sh "docker image tag two-tier-flaskapp:latest ${dockerHubUser}/two-tier-flaskapp:latest"
                sh "docker push ${dockerHubUser}/two-tier-flaskapp:latest"
            }
        }
    }
    stage("Deploy"){
        steps{
            sh "docker compose up -d --build"
        }
    }

}