pipeline {
  agent any 

  tools {
    maven 'maven'
  }

  stages{
    stage('git checkout'){
      steps{
        git branch: 'main', url: 'https://github.com/JOMACS-IT/web-app.git'
      }
    }

    stage('maven build'){
      steps{
        sh 'mvn clean package'
      }
    }

    stage('code analysis'){
           environment {
               scannerHome = tool 'sonar'
           } 
           steps{
               script{
                   withSonarQubeEnv('sonar'){
                       sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=bronzy-webapp"
                   }
               }
          
           }   
        }

        stage('upload to nexus'){
          steps{
            nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: '/var/lib/jenkins/workspace/bronzy-webapp-jenkinsfile/target/web-app.war', type: 'war']], credentialsId: 'nexus-credentials', groupId: 'com.mt', nexusUrl: '13.236.178.34:8081/repo/bronzy-webapp', nexusVersion: 'nexus3', protocol: 'http', repository: 'bronzy-webapp', version: '3.0.6-RELEASE'
          }
        }
  }
}
