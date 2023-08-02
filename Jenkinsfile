#!groovy
def tag="latest"
def dockerHubUser="ayemyak"
def httpPort="9080"

node {
  stages {
  	stage('Checkout') {
	   Checkout scm
      }
        stage('Build') {
      	   sh 'mvn clean install'
    }
	stage('Image Prune') {
	   sh "docker image prune -f"
   }
	stage('Image Build') {
	   sh "docker build -t $containerName:$tag -t $containerName --pull --no--cache ."
	   echo "Image build complete"
  }
	stage('Push to Docker Registry') {
	   withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')])
	 {
	   sh "docker login -u $dockerUser -p $dockerPassword"
	   sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
	   sh "docker push $dockerUser/$containerName:$tag"
  	   echo "Image push complete"
	}
  }
	stage('Run App') {
	   sh "docker rm $containerName -f"
	   sh "docker $dockerHubUser/$containerName"
   	   sh "docker run -d --rm -p $httpPort:$httpPort --name $containerName $dockerHubUser/$containerName:$tag"
	   echo "Application started on port: ${httpPort} (http)"
 }

}
}
