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
            nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: '/var/lib/jenkins/workspace/bronzy-pipeline-jenkins/target/web-app.war', type: 'war']], credentialsId: 'nexus-credentials', groupId: 'com.mt', nexusUrl: '13.236.178.34:8081/repository/bronzy-webapp', nexusVersion: 'nexus3', protocol: 'http', repository: 'bronzy-webapp', version: '3.0.6-RELEASE'
          }
        }

        stage('prod deployment'){
          steps{
            deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://3.25.83.169:8080')], contextPath: null, war: 'target/web-app.war'
          }
        }
  }
}
