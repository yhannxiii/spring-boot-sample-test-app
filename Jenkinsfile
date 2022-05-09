pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'mvn -B -DskipTests clean package'
      }
    }

    stage('test') {
      parallel {
        stage('test') {
          steps {
            bat 'mvn -Dtest="com.example.testingweb.smoke.**" test'
          }
        }

        stage('integretion') {
          steps {
            bat 'mvn -Dtest="com.example.testingweb.integration.**" test'
          }
        }

        stage('Functional') {
          steps {
            bat 'mvn -Dtest="com.example.testingweb.functional.**" test'
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        bat 'mvn -B -DskipTests install'
      }
    }

  }
  tools {
    maven 'maven'
    jdk 'java 11'
  }
}