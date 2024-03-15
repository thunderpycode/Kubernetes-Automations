pipeline {

//   environment {
//     dockerimagename = "nginx:latest"
//     dockerImage = "nginx"
//   }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/thunderpycode/Kubernetes-Automations.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          powershell """
          
          docker run inginx:latest
          """
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentsvc.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}