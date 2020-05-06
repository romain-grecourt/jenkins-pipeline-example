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
                            echo "build !!!"
                            touch ./TEST-io.helidon.build.publisher.model.PipelineRunTest.xml
                            sleep 120
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
                      sh 'echo "copyright!"'
                  }
              },
              'checkstyle': {
                  stage('checkstyle') {
                      sh 'echo "checkstyle!"'
                  }
              }
          ]
          parallel stages
        }
      }
    }
  }
}