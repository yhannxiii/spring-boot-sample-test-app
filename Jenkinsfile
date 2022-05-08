pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        archiveArtifacts(artifacts: '**/target/*.jar', onlyIfSuccessful: true)
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
            sh 'mvn -Dtest="com.example.testingweb.functional.**" test'
          }
        }

      }
    }

    stage('Deploy') {
      when {
        expression {
          currentBuild.result == null || currentBuild.result == 'SUCCESS'
        }

      }
      input {
        message 'Voulez-vous continuer ?'
        id 'Allons-y'
        parameters {
          string(name: 'VALIDATEUR', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        }
      }
      steps {
        echo "Validateur : ${VALIDATEUR}"
        sh 'mvn -B -DskipTests install'
        sh 'java -jar target/testing-web-complete.jar &'
      }
    }

  }
  tools {
    maven 'maven 3.8'
    jdk 'java 11'
  }
}