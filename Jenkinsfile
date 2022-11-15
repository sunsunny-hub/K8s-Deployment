pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                 app = docker.build("k8s-app:${env.BUILD_NUMBER}")
                 sh "docker tag k8s-app:${env.BUILD_NUMBER} 550992133034.dkr.ecr.ap-south-1.amazonaws.com/k8s-app:${env.BUILD_NUMBER}"
                }
            }
        }

        stage('Pushing to ECR') {
            steps{  
                script {
                sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 550992133034.dkr.ecr.ap-south-1.amazonaws.com'
                sh "docker push 550992133034.dkr.ecr.ap-south-1.amazonaws.com/k8s-app:${env.BUILD_NUMBER}"
         }
        }
      }    
        stage('K8S Deploy') {
        steps{   
            sh 'kubectl apply -f  deployment.yaml'
            sh 'kubectl apply -f  service.yaml'
        }
      }
    }
}
