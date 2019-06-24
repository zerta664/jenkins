#!/usr/bin/env groovy
pipeline {
  agent any
   tools { 
        maven 'maven'
    }
    stages {
      stage('Clone sources') {
        steps {
            git branch: 'master',
                url: 'https://github.com/zivkashtan/course.git'

            sh "ls -lat"
        }
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
            sh 'mvn -B -DskipTests clean install -X' 
          }
        }
        stage('archive') {
          steps {
            parallel(
              //"Junit": {
              //   junit 'core/target/surefire-reports/*.xml'
              //},
               "Archive": {
                  archiveArtifacts(artifacts: '**/target/*.jar', onlyIfSuccessful: true, fingerprint: true)
                       
              }
           )
         }
      }
      stage('Publish') {
        steps {
          nexusPublisher nexusInstanceId: 'LocalHexus', nexusRepositoryId: 'company-project', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'web/target/time-tracker-web-0.3.1.war']], mavenCoordinate: [artifactId: 'time-tracker-web-war', groupId: 'tracker', packaging: 'war', version: '0.3.1']]]
        }
     }
  }
}
