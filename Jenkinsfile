pipeline {
  environment {
    dockerimagename = "bravinwasike/react-app"
    dockerImage = ""
    githubCredential = 'github-credentials'
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git url: 'https://github.com/mlowe9999/jenkins-kubernetes-deployment.git',
            credentialsId: "${githubCredential}"  // Use GitHub credentials for checkout
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
          kubernetesDeploy(configs: "deployment.yaml", 
                                         "service.yaml")
        }
      }
    }
  }
}

