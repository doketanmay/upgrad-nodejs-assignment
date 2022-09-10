
pipeline {
    agent any
    environment {
        IMAGE_REPO_NAME="nodejs-app"
        IMAGE_TAG="nodejs-web-app:latest"
        REPOSITORY_URI = "712997521892.dkr.ecr.us-east-1.amazonaws.com/nodejs-app"
    }
   
    stages {
         stage('AWS ECR Login') {
            steps {
                script {
                sh "aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 712997521892.dkr.ecr.us-east-1.amazonaws.com/nodejs-app"
                }                 
            }
        }
        
      stage(' Build Image') {
            steps {
                sh "sudo docker build -t $IMAGE_TAG ."
            }
        }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing image to ECR') {
    steps{  
         script {
                sh "sudo docker tag $IMAGE_TAG $REPOSITORY_URI"
                sh "sudo docker push $REPOSITORY_URI"
         }
        }
      }

    stage('Deploying Container') {
        steps {
            script {
                echo 'Using remote command over ssh'
                sshagent(credentials : ['id_name_added_underManageCredential']){
                    //sh "sudo ssh -i /var/jenkins_home/upgrad.pem ubuntu@10.0.4.181"
                    ssh  -o StrictHostKeyChecking=no ubuntu@10.0.4.181 uptime "whoami" "sudo docker ps -a"
                    //sh "sudo docker ps -a"
                    //sh '''#!/bin/bash
                    //    sudo docker stop $(sudo docker ps -a)
                    //    sudo docker rm $(sudo docker ps -a)
                    //    sudo docker rmi $(sudo docker images -q)
                    //    aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 712997521892.dkr.ecr.us-east-1.amazonaws.com/nodejs-app
                    //    sudo docker run -itd $REPOSITORY_URI
                    //    sudo docker ps
                    //'''
                    }                
                }
            }
        }

    }
}
