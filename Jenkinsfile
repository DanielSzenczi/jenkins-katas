pipeline {
  agent any
  stages {
    stage('Parallel execution') {
      parallel {
        stage('Build') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('Build app') {
          options {
            skipDefaultCheckout true
                  }
          agent {
            docker {
              image 'gradle:jdk11'
            }

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
        stage('test app') {
          options {
            skipDefaultCheckout true
                  }
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          steps {
            unstash 'code'
            sh 'ci/unit-test-app.sh'
            junit 'app/build/test-results/test/TEST-*.xml'
          }
        }
        stage('clone down') {
          steps {
            stash excludes: '/.git/', name: 'code'
          }
        }

      }
    }

  }
  post {
    always {
      deleteDir() /* clean up our workspace */
          }
        }
}
