pipeline {
    environment {
        imagename = "superrepo"
        registry = "027664986317.dkr.ecr.us-east-1.amazonaws.com/superrepo"
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
        
        stage('lint'){
            steps {
                sh "virtualenv --python=/usr/bin/python venv"
                sh "export TERM='linux'"  
                sh 'pylint --rcfile=pylint.cfg funniest/ $(find . -maxdepth 1 -name "*.py" -print) --msg-template="{path}:{line}: [{msg_id}({symbol}), {obj}] {msg}" > pylint.log || echo "pylint exited with $?"'
                sh "rm -r venv/"
               
                echo "linting Success, Generating Report"
                 
                warnings canComputeNew: false, canResolveRelativePaths: false, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', messagesPattern: '', parserConfigurations: [[parserName: 'PyLint', pattern: '*']], unHealthy: ''
               
            }   
        }

        stage('Build') { 
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
        }
    }
      
} 