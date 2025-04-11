pipeline {
  agent any

  stages {

    stage('Clone repository') {
      when {
        branch 'dev'
      }
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      when {
        branch 'dev'
      }
      steps {
        script {
          app = docker.build("vladimirhristovski/kiii-h1")
        }
      }
    }

    stage('Push image') {
      when {
        branch 'dev'
      }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            app.push("${env.BRANCH_NAME}-latest")
          }
        }
      }
    }
  }

  post {
    always {
      echo 'Pipeline completed.'
    }

    success {
      echo 'Pipeline was successful!'
    }

    failure {
      echo 'Pipeline failed.'
    }
  }
}
