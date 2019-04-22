pipeline {
  agent any
  stages {
    stage('Docker maven test') {
      agent {
        docker {
          label 'docker'
          image 'maven:3-alpine'
        }
      }
      steps {
        sh 'mvn --version'
        sh 'ls -la'
        sh 'ls -la /'
      }
    }
  }
} 