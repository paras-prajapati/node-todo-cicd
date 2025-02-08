pipeline {
    agent { label 'node-agent' }
    
    stages{
        stage('Clone'){
            steps{
                echo "git clone"
                git branch: 'master', url: 'https://github.com/paras-prajapati/node-todo-cicd.git'
            }
        }
        stage('Build'){
            steps{
                echo "image build"
                sh 'docker build . -t paras750/node-todo-test:latest'
            }
        }
        stage('Push'){
            steps{
                echo "build code"
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh 'docker push paras750/node-todo-test:latest'
                }
                
            }
        }
        stage("Test"){
            steps{
                echo "test code"
            }
        }
        stage('Deploy'){
            steps{
                echo "deploy code on server"
                sh "docker-compose down && docker-compose up -d --build"
            }
        }
    }
}