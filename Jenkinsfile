
pipeline {
    agent any

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/Bahadur143/two-tier-flask-app.git", branch: "dev"
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flaskapp ."
            }
        }

        stage("Test") {
            steps {
                echo "Test code"
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerHubCreds",
                        usernameVariable: "dockerHubUser",
                        passwordVariable: "dockerHubPass"
                    )
                ]) {
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                    sh "docker image tag two-tier-flaskapp:latest ${dockerHubUser}/two-tier-flaskapp:latest"
                    sh "docker push ${dockerHubUser}/two-tier-flaskapp:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }
}
