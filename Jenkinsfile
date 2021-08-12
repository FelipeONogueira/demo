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
      parallel {
        stage('SonarQube analysis') {
          steps {
            withSonarQubeEnv('sonarqube') {
              sh 'mvn clean package sonar:sonar'
            }

          }
        }

        stage('Wait SonarQube Analysis') {
          steps {
            waitForQualityGate(abortPipeline: true, credentialsId: 'sonarqube', webhookSecretId: '7eb3fb31115252e81a376f1970fb426c81f5702a')
          }
        }

      }
    }

    stage('Clean and install') {
      steps {
        sh 'mvn clean install'
      }
    }

  }
}