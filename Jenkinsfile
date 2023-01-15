pipeline {
  environment {
    imagename = "django-polls"
    ecrurl = "027664986317.dkr.ecr.us-east-1.amazonaws.com"
    ecrcredentials = "ecr:us-east-1:ecr-ismail"
    dockerImage = ''
  } 
  agent any       
  stages {
  
    stage("Fix the permission issue") {
        agent any
        steps {
            sh "sudo chown root:jenkins /run/docker.sock"
        }
    }
  
    stage('Cloning Git') {
      steps {
         checkout scm
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = sudo docker.build imagename
        }
      }
    }
   
  stage('Deploy Master Image') {
    when {
      anyOf {
            branch 'master'
      }
     }
      steps{
        script {
          docker.withRegistry(ecrurl, ecrcredentials) {     
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    
    stage('Remove Unused docker image - Master') {
      when {
      anyOf {
            branch 'master'
      }
     }
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:latest"
      }
    } // End of remove unused docker image for master
  }  
} //end of pipeline