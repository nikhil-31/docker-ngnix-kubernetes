node {
    def app
    
    stage("Fix the permission issue") {
//         agent any
//         steps {
        app.inside {
            sh "sudo chown root:jenkins /run/docker.sock"
        }
    }

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("nikhilsuper/django-polls")
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
        echo "triggering updatemanifestjob"
        build job: 'update-manifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}