pipeline {
  environment {
    dockerimagename = "jettsonoda/react-app"
    dockerImage = ""
    KUBECTL_VERSION = '1.9.0' // Specify the version of kubectl you want to install
    KUBECTL_PATH = "${env.JENKINS_HOME}/tools/kubectl" // Define the path where kubectl will be installed
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

    // stage('Install kubectl') {
    //   steps {
    //     script {
    //       // Create the directory if it doesn't exist
    //       sh "mkdir -p $KUBECTL_PATH"
    //       // Download kubectl
    //       sh "curl -LO https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl"
    //       // Move kubectl to the installation directory
    //       sh "mv kubectl $KUBECTL_PATH"
    //       // Make kubectl executable (optional)
    //       sh "chmod +x $KUBECTL_PATH/kubectl"
    //       // Add kubectl directory to PATH
    //       env.PATH += ":$KUBECTL_PATH"
    //     }
    //   }
    // }

    stage('Deploy to Minikube') {
      steps {
        script {
          // Apply Kubernetes manifests using the installed kubectl
          // sh "$KUBECTL_PATH/kubectl config use-context minikube" 
          // sh "$KUBECTL_PATH/kubectl apply -f deployment.yaml --context default"
          // sh "$KUBECTL_PATH/kubectl apply -f service.yaml --context default"
          sh "kubectl apply -f deployment.yaml --context default"
          sh "kubectl apply -f service.yaml --context default"
        }
      }
    }
  }
}