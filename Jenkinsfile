pipeline {
  environment {
    dockerimagename = "jettsonoda/react-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/ItsJustJett/jenkins-kubernetes-deployment.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
            // Apply deployment configuration
            sh "/usr/local/bin/kubectl apply -f deployment.yaml"

            // Apply service configuration
            sh "/usr/local/bin/kubectl apply -f service.yaml"
        }
      }
    }
  }
}