pipeline {
	agent any
	stages {
		stage("checkout"){
			steps{
				checkout scm
			}
		}
		
		stage("installing dependencies"){
			steps{sh "pip3 install -r requirements.txt"}
		}
		
		stage("Testing stage"){
			steps{sh "pytest"}
		}
		
		stage("Build docker image"){
			steps{sh "docker build -t flask_server ."}
		}
		
		stage("deploy"){
			steps{sh ''' 
			docker rm -f flask_app || true
			
			docker run -d \
			-p 5005:5000 --name flash_app \
			flask_server 
			'''}
		}
	}


}
