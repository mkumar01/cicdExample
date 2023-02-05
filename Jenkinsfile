pipeline {
    agent any
    
    stages{
        stage('Code'){
            steps{
                git url: 'https://github.com/mkumar01/cicdExample.git', branch: 'master' 
            }
        }
        
        stage('Environment Cleanup'){
            steps{
                sh 'docker rmi mkumar0522/node-todo-test'
            }
        }
        stage('Build Docker Image'){
            steps{
                sh 'docker build . -t mkumar0522/node-todo-test:latest'
            }
        }
        stage('Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                 sh 'docker push mkumar0522/node-todo-test:latest'
                }
            }
        }
        stage('Deploy'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
