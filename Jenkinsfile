pipeline{
agent any
environment{
DOCKER_IMAGE="shreyavinay30/shreya-image"

}
stages{
stage('Clone Respository')}
steps{
git 'https://github.com/shreyavinay30/shreyakeeru.git'
}
}
stage('Build Docker Image')}
steps{
script{
docker.build("$(DOCKER_IMAGE):v1")
}
}
}

stage('Login to Docker Hub'){
steps{
   withcredentials([usernamePassword(
    credentialsId: 'dockerhub-creds',
     usernameVarible: 'DOCKER_USER',
passwordVariable: 'DOCKER_PASS'
)])}
 bat 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
}
}
}

stage('Push Docker Image'){
steps{
     script{
               docker.withRegistry('','dockerhub-creds'){
                  docker.image("$(DOCKER_IMAGE):v1").push()
}
}
}
}
}
    post {
          success{
             echo 'Image successfully built and pushed to Docker Hub'
}
   failure{
echo 'Pipeline failed'
}
}
}DOCKER_USER
