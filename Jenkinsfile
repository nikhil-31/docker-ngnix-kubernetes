pipeline {
    environment {
        imagename = "superrepo"
        registry = "027664986317.dkr.ecr.us-east-1.amazonaws.com"
        registryCredential = "ecr:us-east-1:aws"
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
        
//         stage('Docker Build') {
//             agent any
//             steps {
//                 sh 'docker build -t superrepo .'
//             }
//         }
//         
//         stage('Docker Push to DockerHub') {
//             agent any
//             steps {
//                 withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', 
//                                 usernameVariable: 'dockerHubUser')]) {
// 
//                     sh "sudo docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
//                     sh 'sudo docker push nikhilsuper/django-polls:latest'
//             }
//         }

        stage('Build') { 
//             steps { 
//                 sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 027664986317.dkr.ecr.us-east-1.amazonaws.com'
//                 sh 'docker build -t superrepo .'
//             }
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage('Docker Push to ECR') {
            agent any
            steps { 
                script {
                    docker.withRegistry("https://" + registry, registryCredential) {
                        dockerImage.push()
                    }
                }
            }
//             steps {
//                 sh 'docker tag superrepo:latest 027664986317.dkr.ecr.us-east-1.amazonaws.com/superrepo:latest'
//                 sh 'docker push 027664986317.dkr.ecr.us-east-1.amazonaws.com/superrepo:latest'
//             }
        }
    
    }
    
      
} 