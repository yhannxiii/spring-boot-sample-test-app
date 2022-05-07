pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

  }
  tools {
    maven 'maven 3.8'
  }
}