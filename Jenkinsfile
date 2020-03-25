pipeline {
  agent {
    docker { image 'maven:3-alpine' }
  }
  environment {
    FOO = 'bar'
  }
  stages {
    stage('Build') {
      steps {
        sh '''
          echo 'Building :) ${BUILD_ID}' > build.txt\n\
          touch TEST-io.helidon.common.reactive.BaseProcessorTest.xml
          sleep 3
        '''
        junit testResults: 'TEST-io.helidon.common.reactive.BaseProcessorTest.xml', allowEmptyResults: false
      }
    }
    stage('Test') {
      parallel {
        stage('test1') {
          steps {
            sh '''
              echo 'Test1a !!!' > test1a.txt
              sleep 5
            '''
            sh '''
              echo 'Test1b !!!' > test1b.txt\n\
              touch TEST-io.helidon.build.publisher.model.PipelineEventsTest.xml
              sleep 2
            '''
            junit testResults: 'TEST-io.helidon.build.publisher.model.PipelineEventsTest.xml', allowEmptyResults: false
            archiveArtifacts artifacts: 'test1*.txt'
          }
        }
        stage('test2') {
          steps {
            sh '''
              echo 'Test2a !!!' > test2a.txt
              sleep 10
            '''
            sh '''
              echo 'Test2b !!!' > test2b.txt\n\
              touch TEST-io.helidon.build.publisher.model.PipelineRunTest.xml
              sleep 4
            '''
            junit testResults: 'TEST-io.helidon.build.publisher.model.PipelineRunTest.xml', allowEmptyResults: false
            archiveArtifacts artifacts: 'test2*.txt'
          }
        }
      }
    }
  }
}