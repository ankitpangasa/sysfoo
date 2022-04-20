pipeline {
  agent any
  triggers { pollSCM('H/2 * * * *')}
  tools{
      maven 'Maven 3.6.3' 
  }
  
  stages{
      stage("one"){
          steps{
              echo 'Compiling'
              sh 'mvn compile'
          }
      }
      stage("two"){
          steps{
              echo 'Testing...'
              sh 'mvn test'
          }
      }
      stage("three"){
          steps{
               echo 'Packaging...'
               sh 'mvn package -DskipTests'
               archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
          }
      }
  }

  post{
    always{
        echo 'This pipeline is completed..'
    }
  }
}
