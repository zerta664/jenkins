#!/usr/bin/env groovy
pipeline {
  agent any
  
    stages {
      stage('Clone sources') {
        git url: 'https://github.com/zivkashtan/course.git'
      }
    
   // stage('Artifactory configuration') {
        // Tool name from Jenkins configuration
     //   rtMaven.tool = "Maven-3.6.1"
        // Set Artifactory repositories for dependencies resolution and artifacts deployment.
      //  rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
      //  rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    //}
   
        stage('Build') { 
          steps {
            sh 'mvn -B -DskipTests clean package' 
          }
        }
        stage('archive') {
          steps {
            parallel(
              "Junit": {
                 junit 'target/surefire-reports/*.xml'
              },
               "Archive": {
                  archiveArtifacts(artifacts: 'target/*.jar', onlyIfSuccessful: true, fingerprint: true)
                       
              }
           )
         }
      }
    }  
}
