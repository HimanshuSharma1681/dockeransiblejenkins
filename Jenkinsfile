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
        
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u 28071989 -p ${dockerHubPwd}"
                }
                
                sh "docker push 28071989/devops-ansible:${DOCKER_TAG} "
            }
        }
        
        stage('Docker Deploy'){
            steps{
              ansiblePlaybook credentialsId: 'dev-server', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }
    }
}

def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
