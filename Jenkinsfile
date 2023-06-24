pipeline {
  agent any
  stages {

    stage("Cloning Git Repo") {
      steps {
        git branch: 'main', credentialsId: 'personal-GitHub-Creds', url: 'https://github.com/hemant-1905/java-app-image-on-Dockerhub.git'
      }
    }

    stage("building maven build") {
      tools {
        maven 'Maven3.9'
      }
      steps {
        sh 'mvn clean install package'
      }
    }
    stage("Building Docker image") {
      steps {
        script {
          sh 'docker build -t hemaant07/devops-integration .'
        }
      }
    }
    stage("Deploy to DockerHub") {
      steps {
        script {
          withCredentials([string(credentialsId: 'docker_pwd', variable: 'docker_password')]) {
            sh 'docker login -u hemaant07 -p ${docker_password}'
          }
          sh 'docker push hemaant07/devops-integration'
        }
      }
    }

    stage("Delete Existing K8 Objects") {
      steps {
        script {
          def isDeploymentPresent = sh(
            script: 'kubectl get deployment spring-boot-k8s-deployment',
            returnStatus: true
          )

          def isServicePresent = sh(
            script: 'kubectl get service springboot-k8ssvc'
            returnStatus: true
          )

          if (isDeploymentPresent == 0) {
            sh 'kubectl delete deployment spring-boot-k8s-deployment'
          }

          if (isServicePresent == 0) {
            sh 'kubectl delete service springboot-k8ssvc'
          }

        }
      }

    }

    stage("Deploy app to Kubernetes Cluster") {
      steps {
        sh 'kubectl apply -f deploymentservice.yaml'
      }
    }

  }
}