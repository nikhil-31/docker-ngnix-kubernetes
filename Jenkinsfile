pipeline {
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
        
//         stage('lint'){
//             steps {
//                 sh 'docker run -i --rm -e GIT_SRC="$GIT_URL" -e GIT_NAME="$GIT_NAME" -e GIT_BRANCH="$BRANCH_NAME" -e GIT_CHANGE_ID="$CHANGE_ID" eeacms/pylint'
//             }   
//         }

        stage('Docker Build') {
            agent any
            steps {
      	        sh "docker build -t nikhilsuper/django-polls:${BUILD_NUMBER} ."
            }
        }
    
        stage('Docker Push') {
    	    agent any
            steps {
      	        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', 
      	                        usernameVariable: 'dockerHubUser')]) {
                    sh "sudo docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "sudo docker push nikhilsuper/django-polls:${BUILD_NUMBER}"
                }
            }
        }         
        
        stage('Update manifest: Invoke updatemanifest') {
            agent any
            steps {
                build job: 'updatemanifest', parameters: [
                    string(name: 'DOCKERTAG', value: "${BUILD_NUMBER}")
                ]  
            }
        }
    }
      
} 