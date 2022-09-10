
pipeline {
    agent any
    environment {
        IMAGE_REPO_NAME="nodejs-app"
        IMAGE_TAG="nodejs-web-app:latest"
        REPOSITORY_URI = "712997521892.dkr.ecr.us-east-1.amazonaws.com/nodejs-app"
    }
   
    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 712997521892.dkr.ecr.us-east-1.amazonaws.com/nodejs-app"
                }
                 
            }
        }
        
      stage('Git checkout and Build') {
            steps {
                sh "sudo docker build -t $IMAGE_TAG ."
            }
        }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "sudo docker tag $IMAGE_TAG $REPOSITORY_URI/$IMAGE_TAG"

                sh "sudo docker push $IMAGE_TAG $REPOSITORY_URI/$IMAGE_TAG"
         }
        }
      }
    }
}
