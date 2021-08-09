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
      steps{
        withSonarQubeEnv('sonarqube') {
          sh 'mvn clean package sonar:sonar'
        } // submitted SonarQube taskId is automatically attached to the pipeline context
      }
    }

    stage('Clean and install') {
      steps {
          sh 'mvn clean install'
      }
    }
    // stage("Quality Gate"){
    //   steps{
    //     timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
    //       def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
    //       if (qg.status != 'OK') {
    //         error "Pipeline aborted due to quality gate failure: ${qg.status}"
    //       }
    //     }
    //   }
    // }


  }

}