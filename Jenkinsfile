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
        //below line is correct but when we use localhost it doesn't identify on machine, in other cases real ones,this would be the ip, so it works. Also this
        //method does not require artifactory or jfrog plugin as we are using jfrog CLI to connect using commands.
        sh 'jfrog rt upload --url http://127.0.0.1:8083/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/devops-integration.jar java-web-app/'
      }
    }
  }
}