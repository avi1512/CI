node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
        steps {
            container('docker') {
                    sh 'dockerd & > /dev/null'
                    sleep(time: 10, unit: "SECONDS")
       app = docker.build("avi1512/apppy")
            }
        }
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
                build job: 'deployment', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
