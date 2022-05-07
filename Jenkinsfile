pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''mvn -B -DskipTests clean package
'''
      }
    }

    stage('Test') {
      steps {
        sh 'mvn -Dtest="com.example.testingweb.smoke.**" test'
      }
    }

  }
  tools {
    maven 'maven 3.8'
  }
}