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
          		git credentialsId: 'e097bb7a-0f20-4a3f-b873-9219b63db0c6' , branch: "${git_branch}", url: "${git_url}"
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
