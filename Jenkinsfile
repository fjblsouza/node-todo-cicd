pipeline {
    agent { label "master"}
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/fjblsouza/node-todo-cicd.git", branch: "main"
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build . -t test-node-app"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"DockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){ 
                sh "docker tag test-node-app ${env.dockerHubUser}/test-node-app:latest" 
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}" 
                sh "docker push ${env.dockerHubUser}/test-node-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose up -d"
            }
        }
    }
}
