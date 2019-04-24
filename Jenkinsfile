pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        // stage('Test') {
        //     steps {
        //         sh 'mvn test'
        //     }
            // post {
            //     always {
            //         junit 'target/surefire-reports/*.xml'
            //     }
            // }
        // }
        stage('Deliver') {
            steps {
                // sh './jenkins/scripts/deliver.sh'
                sh 'set -x'
                sh 'mvn jar:jar install:install help:evaluate -Dexpression=project.name'
                sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
                sh 'set +x'
            }
        }
        stage("Publish to nexus") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: "nexus3",
                            protocol: "http",
                            nexusUrl: "192.168.0.11:8081",
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: "java-example",
                            credentialsId: "nexus",
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
}
// AN EXAPMPLE SHOULD BE INVESTIGATED
// pipeline {
//     agent {
//         label "master"
//     }
//     tools {
//         // Note: this should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
//         maven "Maven 3.6.0"
//     }
//     environment {
//         // This can be nexus3 or nexus2
//         NEXUS_VERSION = "nexus3"
//         // This can be http or https
//         NEXUS_PROTOCOL = "http"
//         // Where your Nexus is running
//         NEXUS_URL = "192.168.0.11:8081"
//         // Repository where we will upload the artifact
//         NEXUS_REPOSITORY = "java-example"
//         // Jenkins credential id to authenticate to Nexus OSS
//         NEXUS_CREDENTIAL_ID = "nexus"
//     }
//     stages {
//         stage("clone code") {
//             steps {
//                 script {
//                     // Let's clone the source
//                     git 'https://github.com/danielalejandrohc/cargotracker.git';
//                 }
//             }
//         }
//         stage("mvn build") {
//             steps {
//                 script {
//                     // If you are using Windows then you should use "bat" step
//                     // Since unit testing is out of the scope we skip them
//                     sh "mvn package -DskipTests=true"
//                 }
//             }
//         }
//         stage("publish to nexus") {
//             steps {
//                 script {
//                     // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
//                     pom = readMavenPom file: "pom.xml";
//                     // Find built artifact under target folder
//                     filesByGlob = findFiles(glob: "./*.${pom.packaging}");
//                     // Print some info from the artifact found
//                     echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
//                     // Extract the path from the File found
//                     artifactPath = filesByGlob[0].path;
//                     // Assign to a boolean response verifying If the artifact name exists
//                     artifactExists = fileExists artifactPath;
//                     if(artifactExists) {
//                         echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
//                         nexusArtifactUploader(
//                             nexusVersion: "nexus3",
//                             protocol: "http",
//                             nexusUrl: "192.168.0.11:8081",
//                             groupId: pom.groupId,
//                             version: pom.version,
//                             repository: "java-example",
//                             credentialsId: "nexus",
//                             artifacts: [
//                                 // Artifact generated such as .jar, .ear and .war files.
//                                 [artifactId: pom.artifactId,
//                                 classifier: '',
//                                 file: artifactPath,
//                                 type: pom.packaging],
//                                 // Lets upload the pom.xml file for additional information for Transitive dependencies
//                                 [artifactId: pom.artifactId,
//                                 classifier: '',
//                                 file: "pom.xml",
//                                 type: "pom"]
//                             ]
//                         );
//                     } else {
//                         error "*** File: ${artifactPath}, could not be found";
//                     }
//                 }
//             }
//         }
//     }
// }