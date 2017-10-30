pipeline {
  agent any

  stages {
    stage('start backend') {
      steps {
        echo 'start backend....'
        sh("sudo docker run husamay/rps-backend:latest")
      }
    }

    stage('start frontend') {
      steps {
        echo 'start frontend..'
        sh("sudo docker run husamay/rps-frontend:latest")
      }
    }

    stage('Test') {
      steps {
        sh("echo integration test")
      }
    }
}
