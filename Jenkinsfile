pipeline {
    agent {
        label "jenkins-serverless"
    }
    environment {
      ORG               = 'jstrachan'
      APP_NAME          = 'demo-google-nodejs'
    }
    stages {
      stage('CI Build and push snapshot') {
        when {
          branch 'PR-*'
        }
        steps {
          container('nodejs') {
            sh "npm install"
            sh "CI=true DISPLAY=:99 npm test"
          }
        }
      }
      stage('Build Release') {
        when {
          branch 'master'
        }
        steps {
          container('nodejs') {
            // ensure we're not on a detached head
            sh "git checkout master"
            sh "git config --global credential.helper store"

            sh "jx step git credentials"
            // so we can retrieve the version in later steps
            sh "echo \$(jx-release-version) > VERSION"

            sh "npm install"
            sh "CI=true DISPLAY=:99 npm test"
          }
        }
      }
      stage('Promote to Environments') {
        when {
          branch 'master'
        }
        steps {
          dir ('./charts/nodey29') {
            container('nodejs') {
              sh 'echo TODO'
            }
          }
        }
      }
    }
    post {
        always {
            cleanWs()
        }
    }
  }
