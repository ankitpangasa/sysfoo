pipeline {
  agent any
  triggers { pollSCM('H/2 * * * *')}
  tools{
      maven 'Maven 3.6.3' 
  }
  
  stages{
      stage("build"){
          steps{
              echo 'Compiling'
              sh 'mvn compile'
          }
      }
      stage("test"){
          steps{
              echo 'Testing...'
              sh 'mvn test'
          }
      }
      stage("package"){
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
