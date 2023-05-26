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
          sh 'alias'
          withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
           sh 'printenv'
           sh 'sudo docker build -t drdmytro/numeric-app:""$GIT_COMMIT"" .'
           sh 'docker push drdmytro/numeric-app:""$GIT_COMMIT""'
          }
        }
      } 
      stage('Kubernetes Deployment - DEV') {
        steps {
          withKubeConfig([credentialsId: 'k8s']) {
            sh "sed -i 's#replace#siddharth67/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
            sh "kubectl apply -f k8s_deployment_service.yaml"
          }
        }
      } 
    }
}