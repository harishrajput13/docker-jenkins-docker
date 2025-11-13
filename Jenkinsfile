pipeline {
    agent {label 'slave-1'}
    environment {
        DOCKER_IMAGE="harishrajput3/harish-repo:nginx-image-dockerfile"
        DOCKER_CONTAINER_NAME="nginx-cont"
        NEW_IMAGE_NAME="new-nginx-cont"
        DOCKERHUB_USERNAME="harishrajput3"
        DOCKER_PASSWORD= credentials('docker-cred')
        DOCKERHUB_REPO="harish-repo"
        IMAGE_TAG="nginx-cont-by-jenkinsfile"
    }
    stages { 
        stage ('Pull docker image') { 
            steps { 
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-cred') {
                        docker.image("${DOCKER_IMAGE}").pull()
                    }
                }
            }
        }
        stage ('Run Docker Container') { 
            steps {
                script{ 
                    sh '''
                        docker rm -f <container_id> || true
                        docker run -itd -p 80:80 --name ${DOCKER_CONTAINER_NAME} ${DOCKER_IMAGE} 
                    '''
                }
            }
        }
        stage ('Test container') {
            steps {  
                echo "testing docker container is running or not"
                sh 'docker ps | grep ${DOCKER_IMAGE}'
            }
        }
        stage ('Creating image of running container') {
            steps { 
                script { 
                    sh 'docker commit ${DOCKER_CONTAINER_NAME} ${NEW_IMAGE_NAME}'
                }
            }
        }
        stage ('Login in to dockerhub registry') {
            steps {
                script { 
                    sh 'echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin'
                    sh 'docker tag ${NEW_IMAGE_NAME} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:${IMAGE_TAG}'
                }
            }
        }
        stage ('Push image to DockerHub') {
            steps { 
                script { 
                    sh 'docker push ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}'
                }
            }
        }
    }
    post {
        failure { 
            echo "Pipeline is fail"
        }
        success { 
            echo "Pipeline is success" 
        }
    }
}
