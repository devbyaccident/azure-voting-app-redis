pipeline {
   agent {
      label 'docker'
   }

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker compose build')
         }
      }

      stage('Docker Push') {
         steps {
            echo "Runnning in $WORKSPACE"
            dir("$WORKSPACE/azure-vote") {
               script {
                  docker.withRegistry('', 'dockerhub-token-credential') {
                     def image = docker.build('blackdentech/jenkins-k8s:latest')
                     image.push()
                  }
               }
            }
         }
      }
   }
}
