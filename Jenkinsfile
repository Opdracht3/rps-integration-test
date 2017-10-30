pipeline {
  agent any

  stages {
    stage('start backend') {
      steps {
        echo 'start backend....'
        try {
          sh("sudo docker run -d --rm --name backend-container -p 4000:8080 husamay/rps-backend:latest")
          withCredentials([usernamePassword(credentialsId: 'docker-repo', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh("sudo docker login -u=$USERNAME -p=$PASSWORD")
          }
          sh("sudo docker tag husamay/rps-backend:latest husamay/rps-backend:stable")
          sh("sudo docker push husamay/rps-backend:stable")
        } catch (error) {
          sh("echo integration test failed setting stable...")
          sh("sudo docker run -d --rm --name backend-container -p 4000:8080 husamay/rps-backend:stable")
        }
      }
    }

    stage('start frontend') {
      steps {
        echo 'start frontend..'
        try {
          sh("sudo docker run -d --rm --name frontend-container -p 4000:8080 husamay/rps-frontend:latest")
          withCredentials([usernamePassword(credentialsId: 'docker-repo', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh("sudo docker login -u=$USERNAME -p=$PASSWORD")
          }
          sh("sudo docker tag husamay/rps-frontend:latest husamay/rps-frontend:stable")
          sh("sudo docker push husamay/rps-frontend:stable")
        } catch (error) {
          sh("echo integration test failed setting stable...")
          sh("sudo docker run -d --rm --name frontend-container -p 4000:8080 husamay/rps-frontend:stable")
        }
      }
    }

    stage('Test') {
      steps {
        sh("echo integration test")
        sh 'sleep 5s'
        try {
          sh 'sudo docker exec -t backend-container bash -c \'ls -l\''
        } catch (error) {
          sh("echo integration test failed setting stable...")
          sh("sudo docker run -d --rm --name backend-container -p 4000:8080 husamay/rps-backend:stable")
        }
        try {
          sh 'sudo docker exec -t frontend-container bash -c \'ls -l\''
        } catch (error) {
          sh("echo integration test failed setting stable...")
          sh("sudo docker run -d --rm --name frontend-container -p 4000:8080 husamay/rps-frontend:stable")
        }

      }
    }
  }
}
