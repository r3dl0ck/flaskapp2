pipeline {

  environment {
    imagename = "devopstestaccount/api"
    registryCredential = 'dockerhub-id'
    dockerImage = ''
  }

  agent {
    label 'dockerhost-label'
  }

  stages {
    stage('Build') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }

    stage('Test') {
      steps{
        script {
          dockerImage.inside {
            sh '/bin/true'
          }
        }
      }
    }

    stage('Push') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
          }
        }
      }
    }

    stage ('Deploy') {
      steps {
        script {
          build job: 'api-deploy', parameters: [string(name: 'ENVIRONMENT', value: 'dev'), string(name: 'EXTRA_PARAMS', value: '--skip-tags=drain')
        }
      }
    }

  }
}
