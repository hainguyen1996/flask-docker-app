pipeline {

  environment {
    docker_image_name = "cactushai145/flask-docker-app"
    docker_image = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/hainguyen1996/flask-docker-app.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          docker_image = docker.build docker_image_name
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registry_credential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', registry_credential ) {
            docker_image.push("latest")
          }
        }
      }
    }

    stage('Deploying Flask container to Kubernetes') {
      steps {
        withKubeConfig([credentialsId: 'kube-config', serverUrl: 'https://192.168.56.11:6443']) {
          sh 'kubectl apply -f deployment.yaml -f service.yaml'
        }
      }
    }

  }

}