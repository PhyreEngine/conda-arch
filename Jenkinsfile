pipeline {
  agent { label 'conda-build' }
  environment {
    tmpdir = "${pwd tmp: true}"
  }
  stages {
    stage('build') {
      steps {
        lock('conda-index') {
            sh 'if [ -e pre-build.sh ]; then ./pre-build.sh; fi'
            sh '''#!/bin/bash
set -e
conda build . --cache-dir ${tmpdir}
'''
            sh 'if [ -e post-build.sh ]; then ./post-build.sh; fi'
        }
      }
      post {
        failure {
          withCredentials([string(credentialsId: 'email-recipients', variable: 'EMAIL_RECIPIENTS')]) {
            emailext (
                attachLog: true,
                to: "${EMAIL_RECIPIENTS}",
                subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed.
Check console output at ${env.BUILD_URL}."""
            )
          }
        }
      }
    }
  }
}
