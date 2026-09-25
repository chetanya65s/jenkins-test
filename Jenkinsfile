pipeline {
	agent any
	stages {
		stage("checkout"){
			checkout scm
		}
		
		stage("installing dependencies"){
			sh "pip3 install -r requirements.txt"
		}
		
		stage("Testing stage"){
			sh "pytest"
		}
		
		stage("Build docker image"){
			sh "docker build -t flask_server ."
		}
		
		stage("deploy"){
			sh ''' 
			docker rm -f flask_app || true
			
			docker run -d \
			-p 5005:5000 --name flash_app \
			flask_server 
			'''
		}
	}


}
