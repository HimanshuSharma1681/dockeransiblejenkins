pipeline{
    agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }
  }
  environment{
     imageName = "28071989/devops-ansible" 
     registryCredential = 'dockerhubcpad'
     dockerImage =''
  }
    stages{
        
         stage('Build'){
           steps{
        sh 'mvn -B -DskipTests clean package'
      
      }
    }
     stage('test'){
        steps{
           
         sh 'mvn test'
        
        }
       
     }
     stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imageName
        }
      }
    }
     stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imageName:$BUILD_NUMBER"
         sh "docker rmi $imageName:latest"
 
      }
    }
      
    }
}


