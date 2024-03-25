pipeline {
  environment {
    dockerimagename = "jettsonoda/react-app"
    dockerImage = ""
    KUBECTL_VERSION = '1.9' // Specify the version of kubectl you want to install
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
          // Create the directory if it doesn't exist
          sh "mkdir -p $KUBECTL_PATH"
          // Download kubectl
          sh "curl -LO https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/windows/amd64/kubectl.exe"
          // Move kubectl to the installation directory
          sh "mv kubectl.exe $KUBECTL_PATH"
          // Make kubectl executable (optional)
          sh "chmod +x $KUBECTL_PATH/kubectl.exe"
          // Add kubectl directory to PATH
          env.PATH += ":$KUBECTL_PATH"
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