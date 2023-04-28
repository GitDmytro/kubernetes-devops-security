pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true //so that they can be downloaded later
            }
      }
      stage('Unit Tests') {      
            steps {
              sh "mvn test"
            }
        }   
    }
}
