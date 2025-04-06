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
      stage('Start App') {
         steps {
            sh(script: 'docker compose up -d')
         }
      }
      stage('Run Tests') {
         steps {
            sh(script: 'pytest ./tests/test_sample.py')
         }
         post {
            success {
               echo "Tests passed! :)"
            }
            failure {
               echo "Tests failed :("
            }
         }
      }
      stage('Docker Push') {
         steps {
            echo "Runnning in $WORKSPACE"
            dir("$WORKSPACE/azure-vote") {
               script {
                  docker.withRegistry('', 'dockerhub') {
                     def image = docker.build('blackdentech/jenkins-course:2023')
                     image.push()
                  }
               }
            }
         }
      }
   }
   post {
      always {
         sh(script: 'docker compose down')
      }
   }
}
