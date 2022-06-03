
pipeline {
    agent any
    environment {
        registry = 284257319655.dkr.ecr.us-east-1.amazonaws.com/my-ecr-venky
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/venkytatapudi/spring-petclinic.git-jenkins-file.git']]])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
   
    // Uploading Docker images into AWS ECR

 
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 284257319655.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push 284257319655.dkr.ecr.us-east-1.amazonaws.com/my-ecr-venky:latest'
         }
        }
      }
   
         // Stopping Docker containers for cleaner Docker run
     stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
      
    stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 284257319655.dkr.ecr.us-east-.amazonaws.com/my-ecr-venky:latest'
            }
      }
    }
    }
}

