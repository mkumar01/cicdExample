pipeline {
    agent any
    
    stages{
        stage('Git Checkout'){
            steps{
                git url: 'https://github.com/mkumar01/cicdExample.git', branch: 'master' 
            }
        }
        
        stage('Environment Cleanup'){
            steps{
                
               sh 'docker ps -q -f status=exited | xargs --no-run-if-empty docker rm'
               sh 'docker images -q -f dangling=true | xargs --no-run-if-empty docker rmi'
               sh 'docker volume ls -qf dangling=true | xargs -r docker volume rm' 
                
            }
        }
        stage('Containerize Application'){
            steps{
                sh 'docker build . -t mkumar0522/node-todo-test:latest'
            }
        }
     
        stage('Deploy Application to k8 cluster'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
