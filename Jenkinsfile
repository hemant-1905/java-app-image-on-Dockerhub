//This pipeline is used to publish artifacts into jfrog artifactory post configuring "artifactory" plugin in global configuration in Jenkins..

pipeline {
    agent any
    tools {
        maven 'Maven3.9'
    }

    stages {
        stage("Cloning Git Repo") {
            steps {
                git branch: 'main', credentialsId: 'personal-GitHub-Creds', url: 'https://github.com/hemant-1905/java-app-image-on-Dockerhub.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('Push to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server 'my-artifactory'
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "target/*.jar",
                                "target": "java-web-app/build-${BUILD_NUMBER}/"
                            }
                        ]
                    }"""

                    server.upload spec: uploadSpec
                }
            }
        }
    }
}
