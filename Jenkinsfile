pipeline {
  agent any
  stages {

    stage('Clone down'){
    stash excludes: '/.git/', name: 'code'
    }
    stage('Parallel execution') {
      parallel {
        stage('Build') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('Build app') {
          agent {
            docker {
              image 'gradle:jdk11'
            }
            skipDefualtCheckout(true)

          }
          steps {
            unstash 'code'
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh 'ls'
            deleteDir()
            sh 'ls'
          }
        }

      }
    }

  }
}
