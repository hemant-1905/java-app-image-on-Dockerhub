pipeline {
  agent any
  tools {
        maven 'Maven3.9'
      }

  environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Upload to Artifactory') {
      agent {
        docker {
          image 'releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0' 
          reuseNode true
        }
      }
      steps {
        sh 'jfrog rt upload --url http://localhost:8083 --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/devops-integration.jar java-web-app/'
      }
    }
  }
}