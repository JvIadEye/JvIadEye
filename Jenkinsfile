pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('jviadeye-dockerhub')
  }
  stages {
    stage('Initialize'){
      def dockerHome = tool 'jviadeyeDocker'
      env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
    stage('Build') {
      steps {
        sh 'docker build -t jviadeye/nginx_image .'
      }
    }
    stage('Initialize'){
      def dockerHome = tool 'jviadeyeDocker'
      env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
    stage('Scan') {
      steps {
        sh 'docker scan jviadeye/nginx_image'
      }
    }
    stage('Initialize'){
      def dockerHome = tool 'jviadeyeDocker'
      env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
    stage('Publish') {
      steps {
        sh '''
          docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW
          docker push jviadeye/nginx_image
          docker logout
        '''
      }
    }
  }
}
