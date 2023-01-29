pipeline { 
    environment{
       registry="ahmedka007/tpachatprojctbackend"
       registryCredential='ahmedka007-dockerhub'
       dokerImage="tpachatprojctbackend" 
       PATH = "$PATH:/usr/local/bin"
 } 
    
    agent any
    stages {  
       stage("Cloning Project"){
           steps {
            git branch: 'main',
            url: 'https://github.com/AhmedKanoun90/DevopsProject.git'
            echo 'checkout stage'
            }
       } 
       
       stage ('MVN clean') {
         steps {
            sh 'mvn clean -e'
            echo 'Build stage done'
        }
     }
   
      stage("compile Project"){
           steps {
                 sh 'mvn compile -X -e'
                  echo 'compile stage done'
            }
      }
        stage("unit tests"){
            steps {
                  sh 'mvn test'
                  echo 'unit tests stage done'
            }
        }
         stage("mvn Pckage") {
           steps {
                script {
                  sh "mvn package -DskipTests=true"
              }
           }
       } 
         stage("docker build") {
           steps{
           script {
               dockerImage = docker.build registry + ":$BUILD_NUMBER"
              }
           }
         }  
          stage("DockerHub login ") {
              steps{
                  sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u ahmedka007 -p Leader@2021'
            }
          }
         stage("docker push") {
            steps{
              script {
                docker.withRegistry( '', registryCredential ) {
                dockerImage.push()
             }
           }
       }
      }   
            
         stage('Docker-compose file') {

              steps {
                   sh 'docker-compose up -d';
                    sh 'sleep 300'
              
             }  
        }
      
        
  
     

     } 

 
          
       
   }
}
