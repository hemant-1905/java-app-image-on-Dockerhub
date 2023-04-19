pipeline{
    agent any
tools {
  maven 'Maven3.9'
}
stages{
    stage("building maven build"){
        steps{
                sh 'mvn clean install package'
        }

    }

    stage("Building Docker image")
    {
        steps{
            script{
                sh 'docker build -t hemaant07/devops-integration .'
            }
        }
    }

    stage("Deploy to DockerHub")
    {
        steps{
            script{
                withCredentials([string(credentialsId: 'docker_pwd', variable: 'docker_password')]) {
                    sh 'docker login -u hemaant07 -p ${docker_password}'

}
                sh 'docker push hemaant07/devops-integration'
            }
        }
    }

}
}
