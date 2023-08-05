pipeline {
  agent any
  stages {
    stage('verify version') {
      steps {
        sh 'php --version'
      }
    }
    stage('running') {
      steps {
        sh 'php -S localhost:4450 -t "%cd%/public"'
      }
    }
  }
}