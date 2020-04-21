pipeline {
  agent {
    docker { image 'maven:3-alpine' }
  }
  stages {
    stage('default') {
      steps {
        script {
          stages = [
              'build': {
                  stage('build') {
                      script {
                        try {
                          sh '''
                            echo "build (failing)!"
                            touch ./TEST-io.helidon.build.publisher.model.PipelineRunTest.xml
                            sleep 30
                          '''
                        } finally {
                          archiveArtifacts artifacts: "**/*.xml"
                          junit testResults: 'TEST-io.helidon.build.publisher.model.PipelineRunTest.xml'
                        }
                      }
                  }
              },
              'copyright': {
                  stage('copyright') {
                      sh 'echo "copyright!" ; exit 1'
                  }
              },
              'checkstyle': {
                  stage('checkstyle') {
                      sh 'echo "checkstyle!"'
                  }
              }
          ]
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