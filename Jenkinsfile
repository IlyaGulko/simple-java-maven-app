// pipeline {
//   agent any
//   stages {
//     stage('Docker maven test') {
//       agent {
//         docker {
//           label 'docker'
//           image 'maven:3-alpine'
//         }
//       }
//       steps {
//         sh 'mvn --version'
//         sh 'ls -la'
//         sh 'ls -la /'
//       }
//     }
//   }
// }
pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}