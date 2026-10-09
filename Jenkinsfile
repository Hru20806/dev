pipeline{
agent any
environment{
Docker_IMAGE = "hruthviks06/app-image"
}

stages{
stage('Clone Repository') {
steps{
git 'https://github.com/Hru20806/dev.git'
}
}

stage('Build Docker Image') {
steps {
script {
docker.build("${DOCKER_IMAGE}:latest")
}
}
}

stage('Login to Docker Hub') {
steps {
withCredentials([usernamePassword(
    crendentialsId:"dockerhub-creds',
    usernameVariable:'DOCKER_USER',
    passwordVariable: 'DOCKER_PASS'
)]) {
     bat 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
}
}
}

stage('push Docker Image') [
steps{
script{
      docker.withRegistry('','dockerhub-creds') {
      docker.image("${DOCKER_IMAGE}:latest").push()
}
}
}
}
}

post{
success{
echo'Image successfully built and pushed to Docker Hub'
}
failure{
echo 'pipeline failed'
}
}
}



