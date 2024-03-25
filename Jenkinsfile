pipeline {
  environment {
    dockerimagename = "jettsonoda/react-app"
    dockerImage = ""
    KUBECTL_VERSION = '1.9' // Specify the version of kubectl you want to install
    KUBECTL_PATH = '/usr/local/bin/kubectl' // Define the path where kubectl will be installed
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
    // stage('Deploying React.js container to Kubernetes') {
    //   steps {
    //     script {
    //         // Apply deployment configuration
    //         sh "/usr/local/bin/kubectl apply -f deployment.yaml"

    //         // Apply service configuration
    //         sh "/usr/local/bin/kubectl apply -f service.yaml"
    //     }
    //   }
    // }

    stage('Install kubectl') {
      steps {
        script {
          // Download and install kubectl
          sh "curl -LO https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/windows/amd64/kubectl.exe"
          sh "mv kubectl.exe $KUBECTL_PATH"
          // Add kubectl to PATH
          env.PATH += ":${env.JENKINS_HOME}/tools"
        }
      }
    }

    stage('Deploy to Minikube') {
      steps {
        script {
          // Apply Kubernetes manifests using the installed kubectl
          sh "$KUBECTL_PATH apply -f deployment.yaml"
          sh "$KUBECTL_PATH apply -f service.yaml"
        }
      }
    }
  }
}