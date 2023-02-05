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
                exit 0
            }
        }
        stage('Containerize Application'){
            steps{
                sh 'docker build . -t mkumar0522/node-todo-test:latest'
            }
        }
        stage('Push Image to Repository'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                 sh 'docker push mkumar0522/node-todo-test:latest'
                  exit 0  
                }
            }
        }
        stage('Deploy Application to k8 cluster'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
