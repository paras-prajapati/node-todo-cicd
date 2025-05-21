pipeline {
    agent any 
    stages {
        stage('Checkout') { 
            steps {
                git url: 'https://github.com/paras-prajapati/node-todo-cicd.git' , branch: 'master' 
            }
        }
        stage('Build and Test') { 
            steps {
                sh 'docker build . -t paras750/node_app_img:latest' 
            }
        }
        stage('Push') { 
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-token', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
                  sh 'docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}'
                        echo 'Login Completed'
                  sh 'docker push paras750/node_app_img:latest'
                 }
            }
        }
        stage('Deploy') { 
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
