pipeline {
  agent any
  environment {
        DOCKER_HUB = credentials('DOCKER-CREDS')
        //IMAGE_TAG  = 'myorg/scout-demo-service:latest'
        //IMAGE_TO_SCAN='golang:1.11.0-alpine'
        IMAGE_TO_SCAN='tabasumkhan534/demo-app:latest'

  }
  stages {
    stage('verify docker scout') {
      steps {
        sh '''
          docker scout version
          docker version
        '''
      }
    }
    stage('view help') {
      steps {
        sh 'docker scout help'
      }
    }
    stage('Login to Docker Hub') {
      steps {
         // Log into Docker Hub
         sh 'echo $DOCKER_HUB_PSW | docker login -u $DOCKER_HUB_USR --password-stdin'
        //sh 'echo $DOCKER_ACCESS_TOKEN | docker login -u $DOCKER_ACCESS_USER --password $DOCKER_ACCESS_TOKEN'
      }
    }
    stage('pull a problematic image') {
      steps {
        sh "docker pull $IMAGE_TO_SCAN"
      }
    }
    stage('scan the image') {
      steps {
        sh "docker scout cves --exit-code --only-severity high $IMAGE_TO_SCAN"
      }
    }
    stage('cves help') {
      steps {
        sh 'docker scout cves --help'
      }
    }  
    stage('view the recommendations') {
      steps {
        sh "docker scout recommendations $IMAGE_TO_SCAN"
      }
    }
    stage('trivy scan') {
      steps{
        sh "trivy $IMAGE_TO_SCAN"
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
