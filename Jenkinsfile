pipeline {
  agent none
  environment {
    FOO = 'bar'
  }
  stages {
    stage('Build') {
      agent {
        docker { image 'maven:3-alpine' }
      }
      steps {
        sh '''
          echo 'Building :) ${BUILD_ID}' > build.txt
        '''
      }
    }
    stage('Test') {
      agent {
        docker { image 'node:7-alpine' }
      }
      steps {
        sh '''
          echo 'Testing :) ${BUILD_ID}' > test.txt
        '''
        archiveArtifacts artifacts: 'test.txt'
      }
    }
  }
}
