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
        script {
          testStages = [
            "test1" : stage('test1') {
              sh '''
                echo 'Test1 !!!' > test.txt
              '''
              archiveArtifacts artifacts: 'test1.txt'
            }
          ]
          parallel testStages
        }
      }
    }
  }
}
