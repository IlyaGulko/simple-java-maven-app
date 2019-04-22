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
// pipeline {
//     agent {
//       label 'ssh_agent'
//     }
//     tools {
//         maven 'maven-latest'
//         jdk 'jdk8'
//     }
//     stages {
//         stage ('Initialize') {
//             steps {
//                 sh '''
//                     echo "PATH = ${PATH}"
//                     echo "M2_HOME = ${M2_HOME}"
//                     ls -la 
//                     ls -la /
//                 '''
//             }
//         }

//         // stage ('Build') {
//         //     steps {
//         //         sh 'mvn -Dmaven.test.failure.ignore=true install' 
//         //     }
//         //     post {
//         //         success {
//         //             junit 'target/surefire-reports/**/*.xml' 
//         //         }
//         //     }
//         // }
//     }
// }
