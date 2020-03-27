pipeline {
  agent {
    docker { image 'maven:3-alpine' }
  }
  stages {
    stage('default') {
      parallel {
        stage('build'){
          steps {
            try {
              sh '''
                echo "build (failing)!"
                exit 1
              '''
            } finally {
              junit testResults: 'TEST-io.helidon.build.publisher.model.PipelineRunTest.xml'
            }
          }
        }
        stage('copyright'){
          steps {
            sh 'echo "copyright!"'
          }
        }
        stage('checkstyle'){
          steps {
            sh 'echo "checkstyle!"'
          }
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