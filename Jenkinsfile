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
          echo 'Building :) ${BUILD_ID}' > build.txt\n\
          touch TEST-io.helidon.common.reactive.BaseProcessorTest.xml
        '''
        junit testResults: 'TEST-io.helidon.common.reactive.BaseProcessorTest.xml', allowEmptyResults: false
      }
    }
    stage('Test') {
      agent {
        docker { image 'node:7-alpine' }
      }
      steps {
        sh 'echo blah'
        script {
          testStages = [
            "test1" : {
              stage('test1') {
                sh '''
                  echo 'Test1a !!!' > test1a.txt
                '''
                sh '''
                  echo 'Test1b !!!' > test1b.txt\n\
                  touch TEST-io.helidon.build.publisher.model.PipelineEventsTest.xml
                '''
                junit testResults: 'TEST-io.helidon.build.publisher.model.PipelineEventsTest.xml', allowEmptyResults: false
                archiveArtifacts artifacts: 'test1*.txt'
              }
            },
            "test2" : {
              stage('test2') {
                sh '''
                  echo 'Test2a !!!' > test2a.txt
                '''
                sh '''
                  echo 'Test2b !!!' > test2b.txt\n\
                  touch TEST-io.helidon.build.publisher.model.PipelineRunTest.xml
                '''
                junit testResults: 'ATEST-io.helidon.build.publisher.model.PipelineRunTest.xml', allowEmptyResults: false
                archiveArtifacts artifacts: 'test2*.txt'
              }
            }
          ]
          sh 'echo duh'
          parallel testStages
          sh 'echo duh'
        }
        sh 'echo yeah-duh'
      }
    }
  }
}