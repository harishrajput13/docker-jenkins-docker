pipeline {
    agent {label 'slave-1'}
    environment {
        DOCKER_IMAGE="harishrajput3/harish-repo:nginx-image-dockerfile"
        DOCKER_CONTAINER_NAME="nginx-cont"
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
                    sh 'docker run -itd -p 80:80 --name nginx-cont ${DOCKER_IMAGE}' 
                }
            }
        }
        stage ('Test container') {
            steps {  
                echo "testing docker container is running or not"
                sh 'docker ps | grep ${DOCKER_CONTAINER_NAME}'
            }
        }
    }
    post {
        always {
            echo "Pipeline execution completed"
        }
        failure {
            echo "Pipeline failed "
        }
        success {
            echo "Container deployed successfully"
        }
    }
}
