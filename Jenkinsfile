pipeline {
  agent {
    label 'docker' 
  }
  stages {
    stage('Docker node test') {
      agent {
        docker {
          label 'docker'
          image 'node:7-alpine'
          args '--name docker-node'
        }
      }
      steps {
        sh 'node --version'
      }
    }

    stage('Docker maven test') {
      agent {
        docker {
          label 'docker'
          image 'maven:3-alpine'
        }
      }
      steps {
        sh 'mvn --version'
      }
    }
  }
} 