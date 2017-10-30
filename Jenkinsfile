pipeline {
  triggers {
    upstream(upstreamProjects: "rps-backend", threshold: hudson.model.Result.SUCCESS,
              upstreamProjects: "rps-frontend", threshold: hudson.model.Result.SUCCESS)
  }
  agent any

  stages {

    stage('Clean up') {
      steps {
        sh 'sudo docker stop frontend-container || true'
        sh 'sudo docker rm frontend-container || true'

        sh 'sudo docker stop backend-container || true'
        sh 'sudo docker rm backend-container || true'
      }
    }

    stage('start backend') {
      steps {
        echo 'start backend....'
        sh("sudo docker run -d --rm --name backend-container -p 4000:8080 husamay/rps-backend:latest")
        withCredentials([usernamePassword(credentialsId: 'docker-repo', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh("sudo docker login -u=$USERNAME -p=$PASSWORD")
        }
        sh("sudo docker tag husamay/rps-backend:latest husamay/rps-backend:stable")
        sh("sudo docker push husamay/rps-backend:stable")
      }
    }

    stage('start frontend') {
      steps {
        echo 'start frontend..'
        sh("sudo docker run -d --rm --name frontend-container -p 80:80 husamay/rps-frontend:latest")
        withCredentials([usernamePassword(credentialsId: 'docker-repo', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh("sudo docker login -u=$USERNAME -p=$PASSWORD")
        }
        sh("sudo docker tag husamay/rps-frontend:latest husamay/rps-frontend:stable")
        sh("sudo docker push husamay/rps-frontend:stable")
      }
    }

    stage('Test') {
      steps {
        sh("echo integration test")
        sh 'sleep 5s'
        sh 'sudo docker exec -t backend-container bash -c \'ls -l\''
        sh 'sudo docker exec -t frontend-container bash -c \'ls -l\''

      }
    }
  }


  post {
    failure {
      sh("sudo docker run -d --rm --name frontend-container -p 4000:8080 husamay/rps-frontend:stable")
      sh("sudo docker run -d --rm --name frontend-container -p 4000:8080 husamay/rps-frontend:stable")
    }
  }
}
