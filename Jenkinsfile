pipeline {
	agent none
  stages {
  	stage('Maven Install') {
    	agent {
      	docker {
        	image 'maven:3.5.0'
        }
      }
      steps {
      	sh 'mvn clean install'
      }
    }
    stage('Build') {
      steps {
        sh 'docker build -t jviadeye/nginx_image .'
      }
    }
    stage('Scan') {
      steps {
        sh 'docker scan jviadeye/nginx_image'
      }
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
