pipeline { 
    environment{
       registry="esprituser/tpachatprojctbackend"
       registryCredential='esprituser-dockerhub'
       dokerImage="tpachatprojctbackend" 
       PATH = "$PATH:/usr/local/bin"
 } 
    
    agent any
    stages {  
       stage("Cloning Project"){
           steps {
            git branch: 'master',
            url: 'https://github.com/riadh70/Devops_Project.git'
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
                  sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u esprituser -p P@ssw0rd@imc'
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
         //   stage('Cleaning up') {
        //     steps{
        //     sh "docker rmi $registry:$BUILD_NUMBER"
        //   }
       // }
       stage("SonarQube Analysis") {
         steps {
             withSonarQubeEnv('sq1') {
              sh 'mvn sonar:sonar'
             }
                 
          }
       } 
        
  
     stage("Upload Jar  To Nexus") {
            steps {  
               nexusArtifactUploader artifacts: [ 
                 [ 
                    artifactId: 'tpAchatProject',  
                      classifier: '',  
                      file: 'target/tpAchatProject-1.0.jar',   
                      type: 'jar' 
                   ]  

            ],  
            credentialsId: 'nexus3', 
            groupId: 'com.esprit.examen', 
            nexusUrl: '172.20.10.5:8081', 
            nexusVersion: 'nexus3', 
            protocol: 'http', 
            repository: 'deploymentRepo',  
            version: '1.0' 


        }  

     } 

 
          
       
   }
}
