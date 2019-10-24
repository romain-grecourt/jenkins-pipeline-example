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
            "test1" : {
              stage('test1') {
                sh '''
                  echo 'Test1a !!!' > test1a.txt
                '''
                sh '''
                  echo 'Test1b !!!' > test1b.txt
                '''
                archiveArtifacts artifacts: 'test1*.txt'
              }
            },
            "test2" : {
              stage('test2') {
                sh '''
                  echo 'Test2a !!!' > test2a.txt
                '''
                sh '''
                  echo 'Test2b !!!' > test2b.txt
                '''
                archiveArtifacts artifacts: 'test2*.txt'
              }
            }
          ]
          parallel testStages
          sh 'echo duh'
        }
        sh 'echo yeah-duh'
      }
    }
  }
}
