pipeline {
    agent any
    stages {
        stage ('git checkout') {
            steps {
              git branch: 'main', url: 'https://github.com/sai3222/jenkins-maven.git'
            }
        }    
        stage ('unit testing') {
            steps {
                sh 'mvn test'
            }
        }    
        stage ('Integration Testing') {
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }    
        stage ('Maven Build') {
            steps {
                sh 'mvn clean install'
            }    
        }
        stage ('SonarQube-analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                      sh 'mvn clean package sonar:sonar'    
                }
                }    
            }
        }
      stage ('Quality Gate status') {
            steps {
                script {
                  waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
                }    
            }
      }
      stage ('Upload jar nexus') {
          steps {
              script {
                nexusArtifactUploader artifacts: 
                [
                     [
                            artifactId: 'my-app', 
                            classifier: '', file: 'target/*.jar',
                            type: 'jar'
                            ]
                ], 
                credentialsId: 'nexus-auth', 
                groupId: 'com.example', 
                nexusUrl: '13.59.167.75:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'secound-repo-nexus/', 
                version: '1.0'  
              }
          }
      }    
   }     
}
