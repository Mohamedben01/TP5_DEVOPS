pipeline {
  agent any
  environment{
      DATE = new Date().format('yy.M')
      TAG = "${env.DATE}.${env.BUILD_NUMBER}"
      DOCKER_IMAGE = "mohamed01ben/tp5devops:${TAG}"
  }
  stages {
    stage('Cloning Git') {
      steps {
        git branch: 'master', url: 'https://github.com/Mohamedben01/TP5_DEVOPS'
      }
    }
    stage('Build & Push Docker Image to DockerHub') {
      steps{
        script {
          sh 'docker build -t ${DOCKER_IMAGE} .'
          withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
              sh 'docker login -u mohamed01ben -p ${dockerhubpwd}'
          }
          sh 'docker push ${DOCKER_IMAGE}'
        }
      }
    }
    stage('Deploy image') {
      steps{
          sh 'docker stop tp5devops | true'
          sh 'docker rm tp5devops | true'
          sh 'docker run --name tp5devops -d -p 8181:8181 ${DOCKER_IMAGE}'
      }
    }
  }
}
