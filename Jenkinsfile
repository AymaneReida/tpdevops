pipeline {
    agent any

    environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "user_id/repo"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = ''
        dockerImage = ''
    }

    stages {
        stage('Cloning Git') {
            steps {
            }
        }

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
            sh 'docker ps -f name=myrepoContainer -q | xargs -r docker container stop'
            sh 'docker container ls -a -fname=myrepoContainer -q | xargs -r docker container rm'
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