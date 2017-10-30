pipeline {
  agent any

  stages {
    stage('start backend') {
      steps {
        echo 'start backend....'
        sh("sudo docker run -d -rm -p 4000:8080 husamay/rps-backend:latest")
      }
    }

    stage('start frontend') {
      steps {
        echo 'start frontend..'
        sh("sudo docker run -d -rm -p 80:80 husamay/rps-frontend:latest")
      }
    }

    stage('Test') {
      steps {
        sh("echo integration test")
      }
    }
  }
}
