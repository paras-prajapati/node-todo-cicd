pipeline {
    agent { label 'agent-01-label' }

    environment {
        DOCKER_IMAGE = 'paras750/node_app_img:latest'
        REPO_URL = 'https://github.com/paras-prajapati/node-todo-cicd.git'
        BRANCH = 'master'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${REPO_URL}", branch: "${BRANCH}"
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build . -t ${DOCKER_IMAGE}'
                }
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-token', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
                    sh 'docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}'
                    echo 'Docker login completed'
                    sh 'docker push ${DOCKER_IMAGE}'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy using Docker Compose
                    sh 'docker-compose down && docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            // Clean up only temporary files if needed (e.g., Docker build cache)
            // Keep the main workspace intact
            cleanWs()  // Cleans up files created during the pipeline but not the entire workspace
        }

        success {
            echo "Pipeline completed successfully!"
        }

        failure {
            echo "Pipeline failed. Please check the logs."
        }
    }
}
