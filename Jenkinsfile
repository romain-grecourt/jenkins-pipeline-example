pipeline {
  agent any
  environment {
    FOO = 'bar'
  }
  stages {
    stage('Build') {
      steps {
        sh '''
          echo 'Building :) ${BUILD_ID}' > build.txt
        '''
      }
    }
    stage('Test') {
      steps {
        sh '''
          echo 'Testing :) ${BUILD_ID}' > test.txt
        '''
        archiveArtifacts artifacts: 'test.txt'
      }
    }
  }
}
