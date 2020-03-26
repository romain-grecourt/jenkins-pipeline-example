pipeline {
  agent {
    docker { image 'maven:3-alpine' }
  }
  stages {
    stage('default') {
      parallel {
        stage('build'){
            sh '''
                echo "build (failing)!"
                exit 1
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