pipeline {
  agent any
  stages {
    stage('Environment Version') {
      steps {
        sh 'git --version'
        sh 'mvn --version'
        sh 'java -version'
      }
    }

    stage('SonarQube analysis') {
      steps {
        withSonarQubeEnv('sonarqube') {
          sh 'mvn clean package sonar:sonar'
        }
      }
    }

    stage('Wait SonarQube Quality Gate') {
      steps {
        waitForQualityGate abortPipeline: true     
      }
    }

    stage('Clean and install') {
      steps {
        sh 'mvn clean install'
      }
    }

  }
}