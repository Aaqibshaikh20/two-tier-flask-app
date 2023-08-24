pipeline{
    agent {label 'Slave1'}
    
    stages{
        stage("Clone the Code"){
            steps{
                git url:"https://github.com/Aaqibshaikh20/two-tier-flask-app.git", branch:"master"
            }
        }
        stage("Build the Code"){
            steps{
                sh "docker build . -t two-tier-flask-app-new"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag two-tier-flask-app-new ${env.dockerHubUser}/two-tier-flask-app-new:latest"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app-new:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
