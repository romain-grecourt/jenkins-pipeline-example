pipeline {
  agent {
    docker { image 'maven:3-alpine' }
  }
  stages {
    stage('declarative') {
      parallel {
        stage('build'){
          steps{
            sh '''
                echo "build (failing)!"
            '''
            sh '''
                echo "copyright!"
            '''
            sh '''
                echo "checkstyle!"
            '''
          }
        }
      }
    }
    stage('scripted') {
      steps {
        script {
          testStages = [
            "test1" : {
              stage('test1') {
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
        }
      }
    }
    stage('release') {
      when {
        branch '**/release-*'
      }
      steps {
        sh '''
            echo "release!"
        '''
      }
    }
  }
}