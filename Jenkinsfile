pipeline {
  agent { label 'slave-1' }

  environment {
    DOCKERHUB_REPO = 'harishrajput3/harish-repo' 
    IMAGE_TAG = "build-${env.BUILD_NUMBER}"
    DOCKER_CRED_ID = 'docker-hub-cred'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Image') {
      steps {
        script {
          sh 'docker build -t ${DOCKERHUB_REPO}:${IMAGE_TAG} .'
        }
      }
    }

    stage('Test Image') {
      steps {
        script {
          sh '''
            CID=$(docker run -d -p 8080:80 ${DOCKERHUB_REPO}:${IMAGE_TAG})
            docker ps | grep $CID 
            docker rm -f $CID || true
          '''
        }
      }
    }

    stage('Login & Push') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', DOCKER_CRED_ID) {
            sh "docker push ${DOCKERHUB_REPO}:${IMAGE_TAG}"
          }
        }
      }
    }

    stage('Cleanup') {
      steps {
        script {
          sh "docker rmi ${DOCKERHUB_REPO}:${IMAGE_TAG} || true"
        }
      }
    }
  }

  post {
    success {
      echo "Image ${DOCKERHUB_REPO}:${IMAGE_TAG} pushed successfully."
    }
    failure {
      echo "Pipeline failed. Check logs."
    }
  }
}
