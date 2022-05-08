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
      parallel {
        stage('Unit') {
          steps {
            sh 'mvn -Dtest="com.example.testingweb.smoke.**" test'
            junit 'target/surefire-reports/*.xml'
          }
        }

        stage('Integration') {
          steps {
            sh 'mvn -Dtest="com.example.testingweb.integration.**" test'
          }
        }

        stage('Functional') {
          steps {
            sh '''mvn -Dtest="com.example.testingweb.functional.**" test
'''
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        sh 'mvn -B -DskipTests install'
        sh 'java -jar target/testing-web-complete.jar'
        dir(path: 'target') {
          archiveArtifacts(artifacts: '*', onlyIfSuccessful: true)
        }

      }
    }

  }
  tools {
    maven 'maven 3.8'
    jdk 'java 11'
  }
}