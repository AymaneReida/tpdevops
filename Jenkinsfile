pipeline {
    agent any

    environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "aymaner/tpdevops"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = ''
        dockerImage = ''
    }

    stages {
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }

     // Uploading Docker images into Docker Hub
    stage('Upload Image') {
     steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            }
        }
      }
    }

     // Stopping Docker containers for cleaner Docker run
     stage('docker stop container') {
         steps {
            sh 'docker container ls -aq -f name=myrepoContainer | xargs -r docker container stop'
         }
       }


    // Running Docker container, make sure port 80 is opened in
    stage('Docker Run') {
     steps{
         script {
            dockerImage.run("-p 80:80 --rm --name myrepoContainer")
         }
      }
    }
  }
}