pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'Compiling'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'Testing...'
        sh 'mvn test'
      }
    }

    stage('Build Artifact'){
       when {
        branch 'master'
      }
      parallel{
        stage('package') {
          agent {
            docker {
              image 'maven:3.6.3-jdk-11-slim'
          }
        }
        steps {
          echo 'Packaging...'
          sh 'mvn package -DskipTests'
          archiveArtifacts 'target/*.war'
        }
      }

      stage('Docker BnP.') {
        agent any
        when {
          branch 'master'
        }
        steps {
          script {
            docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
              def dockerImage = docker.build("ankitpangasa/sysfoo:v${env.BUILD_ID}", "./")
              dockerImage.push()
              dockerImage.push("latest")
              dockerImage.push("dev")
            }
          }
        }
      }
    }
  }
  
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
  triggers {
    pollSCM('H/2 * * * *')
  }
}
