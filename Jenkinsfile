
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
                sh "sudo docker push $REPOSITORY_URI:$BUILD_NUMBER"
         }
        }
      }

    stage('Deploying Container') {
         steps {
                sh '''#!/bin/bash
                    sudo ssh -i /var/jenkins_home/upgrad.pem ubuntu@10.0.4.181 << EOF
                    sudo docker ps 
                    sudo docker ps -aq | sudo xargs docker stop | sudo xargs docker rm
                    aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 712997521892.dkr.ecr.us-east-1.amazonaws.com/nodejs-app
                    sudo docker run -itd -p 8080:8080 --env "--prefix=/app" --name apps $REPOSITORY_URI:$BUILD_NUMBER
                    sudo docker ps
                '''
            }
        }
    }

}
