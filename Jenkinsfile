pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''
          echo 'Building :) ${BUILD_ID}' > build.txt\n\
          touch TEST-io.helidon.common.reactive.BaseProcessorTest.xml
          sleep 3
        '''
        stash includes: '*.txt', name: 'build'
        archiveArtifacts artifacts: '*.txt'
        junit testResults: 'TEST-io.helidon.common.reactive.BaseProcessorTest.xml', allowEmptyResults: false
      }
    }
    stage('Test') {
      steps {
        sh 'echo blah ; sleep 2'
        script {
          testStages = [
            "test1" : {
              stage('test1') {
                unstash 'build'
                sh '''
                  cat build.txt
                '''
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
            },
            "test2" : {
              stage('test2') {
                unstash 'build'
                sh '''
                  cat build.txt
                '''
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
          ]
          sh 'echo duh ; sleep 2'
          parallel testStages
          sh 'echo duh ; sleep 3'
        }
        sh 'echo yeah-duh ; sleep 1'
      }
    }
  }
}