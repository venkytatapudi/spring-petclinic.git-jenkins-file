pipeline {
    agent any
    environment {
        registry = "284257319655.dkr.ecr.us-east-1.amazonaws.com/my-ecr-venky"
    }
    stages {
        stage('gitclone') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/venkytatapudi/spring-petclinic.git-jenkins-file.git']]])
            }
        }
        stage('BUILD') {
            steps {
                echo 'Build the code'
                sh './mvnw package'
            }
       }
        stage('Building image') {
            
      steps{
          
        script {
            
          dockerImage = docker.build registry
        }
        
          }
          
        }
        stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 284257319655.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push 284257319655.dkr.ecr.us-east-1.amazonaws.com/my-ecr-venky:latest'
           }
        }
    
       }
          stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=7a1b25e22de2 -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=7a1b25e22de2 -q | xargs -r docker container rm'
         }
       }
       stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 5000:8090 --rm --name 7a1b25e22de2  284257319655.dkr.ecr.us-east-1.amazonaws.com/my-ecr-venky:latest'
            }
      }
      
   }
    }
}
        
