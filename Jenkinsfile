pipeline {
  agent { label 'jenkins-agent' }
  tools {
    jdk 'java17'
    maven 'maven3'
  }
  stages {
    stage("Cleanup Workspace"){
      steps {
        cleanWs()
      }
    }
    stage("Checkout from SCM"){
      steps {
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/mohyor/jenkins-tutorial'
      }
    }

    stage("Build Application"){
      steps {
        sh "mvn clean package"
      }
    }
    
    stage("Test Application"){
      steps {
        sh "mvn test"
      }
    }

    stage("SonarQube Analysis"){
      steps {
        script {
         withSonarQubeEnv(credentialsId: 'jenkins-master') {
           sh "mvn sonar:sonar"
         }
        }
      }
    }
  }
}
