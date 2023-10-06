pipeline {

  agent none


  stages{
      stage('Build'){
          agent{
            docker{
              image 'python:alpine3.17'
              args '--user root'
            }
          }
          steps{
            echo 'Compiling vote app'
            dir('vote'){
              sh 'pip install -r requirements.txt'
            }
          }

      }
      stage('Unit Test'){
          agent{
            docker{
              image 'python:alpine3.17'
              args '--user root'
            }
          }
          steps{
            echo 'Running Unit Tests on vote app'
            dir('vote'){
              sh 'pip install -r requirements.txt'
              sh 'nosetests -v'
            }
          }
      }
      stage('Docker BnP'){
          agent any
          when{
            branch "master"
          }
          steps{
            echo 'Packaging vote app with docker'
            script{
              docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                  def voteImage = docker.build("initcron/vote:v${env.BUILD_ID}", "./vote")
                  voteImage.push()
                  voteImage.push("dev")
              }
            }
          }
      }

  }

  post{
    always{
        echo 'Pipeline for vote is complete..'
    }
 
  }

}
