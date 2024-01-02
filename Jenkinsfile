pipeline {
  agent { label 'jenkins-agent' }
  tools {
    jdk 'java17'
    maven 'maven3'
  }
  stages {
    stage("Cleanp Workspace"){
      steps {
        cleanWs()
      }
    }
    stage("Checkout from SCM"){
      steps {
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/mohyor/jenkins-app'
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
  }
}