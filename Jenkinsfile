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
        script{
          def qualitygate = waitForQualityGate()
          if (qualitygate.status != "OK") {
            error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
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