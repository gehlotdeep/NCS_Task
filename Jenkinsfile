pipeline {
    agent { label "dev-server"}
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/gehlotdeep/NCS_Task.git", branch: "master"
                echo 'Code Cloning'
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-test ."
                echo 'code builded'
            }
        }
        stage("scan image"){
            steps{
                echo 'image scanning'
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'image pushed'
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment'
            }
        }
    }
}
