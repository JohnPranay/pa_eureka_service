pipeline {
	environment {
		git_url = "https://github.com/JohnPranay/pa_eureka_service.git"
		git_branch = "master"
		image_name = "eureka-service"
		image_tag = "master-latest"
		dockerhub_repo = "johnpranay"
		mavenHome= "/usr/share/maven"
	}
	
 agent {label 'docker'}
   stages {
	stage('Pull Source') {
		steps {
          		git credentialsId: '1ae23898-5369-4063-98a0-bcc7d2dc3b41' , branch: "${git_branch}", url: "${git_url}"
		}
	}

	stage('Maven Build') {
		steps {
			sh "${mavenHome}/bin/mvn clean package && cp target/*.jar ."
		}
	}
	
	stage('Docker Build') {
		steps {
			sh "docker build --no-cache -t ${dockerhub_repo}/${image_name}:${image_tag} ."
		}
	}
	
	stage('Docker Push') {
		steps {
			withDockerRegistry([ credentialsId: "dockerhub", url: "" ]){
                	sh "docker push ${dockerhub_repo}/${image_name}:${image_tag}"
			}
		}
	}
  }
  post {
  	always{
		deleteDir() /* clean the workspace*/
	}
  }
}
