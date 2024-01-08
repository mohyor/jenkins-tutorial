pipeline {
  agent { label 'jenkins-agent' }
  tools {
    jdk 'Java17'
    maven 'Maven3'
  }
  environment {
    APP_NAME = "register-app-pipeline"
    RELEASE = "1.0.0"
    DOCKER_USER = "mohyor"
    DOCKER_PASS = 'firebird14'
    IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
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

    stage("Quality Gate"){
      steps {
        waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-master'
      }
    }
    
    stage("Build & Push Docker Image"){
      steps {
        script {
          docker.withRegistry('', DOCKER_PASS) {
            docker_image = docker.build "${IMAGE_NAME}"
          }
          docker.withRegistry('', DOCKER_PASS) {
            docker_image.push("${IMAGE_TAG}")
            docker_image.push('latest')
          }
        }
      }
    }
  }
}
