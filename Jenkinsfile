pipeline{
    agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }
  }
    environment {
      DOCKER_TAG = getVersion()
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
        post{
           always {
              junit 'target/surefire-reports/*.xml'
           }
        }
     }
       
     
        stage('Docker Build'){
            steps{
                sh "docker build . -t 28071989/devops-ansible:${DOCKER_TAG} "
            }
        }
      
    }
}

def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
