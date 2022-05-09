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
        stage('unit') {
          steps {
            bat 'mvn -Dtest="com.example.testingweb.smoke.**" test'
            junit(testResults: 'target/surfire-reports/*.xml', allowEmptyResults: true)
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
        dir(path: 'target') {
          archiveArtifacts(onlyIfSuccessful: true, artifacts: '*')
        }

      }
    }

  }
  tools {
    maven 'maven'
    jdk 'java 11'
  }
}