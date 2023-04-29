pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
             // archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true //so that they can be downloaded later
            }
      }
      stage('Unit Tests - JUnit and Jacoco') {
        steps {
          sh "mvn test"
        }
        post {
          always {
            junit 'target/surefire-reports/*.xml'
            jacoco execPattern: 'target/jacoco.exec'
          }
        }
      } 
      stage('Docker Build and Push') {
        steps {
          withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
           sh 'printenv'
           sh 'sudo crictl build -t drdmytro/numeric-app:""$GIT_COMMIT"" .'
           sh 'crictl push drdmytro/numeric-app:""$GIT_COMMIT""'
          }
        }
      }  
    }
}