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
  }
}
