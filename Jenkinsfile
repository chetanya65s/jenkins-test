pipeline {
	agent any
	stages {
		stage("checkout"){
			steps{
				checkout scm
			}
		}
		
		stage("installing dependencies"){
			steps{sh '''
				python3 -m venv venv
				  ./venv/bin/pip install -r requirements.txt'''}
		}
		
		stage("Testing stage"){
			steps{sh "./venv/bin/pytest"}
		}
		
		stage("Build docker image"){
			steps{sh '''
			docker build -t chetanyagarg/flask_server:latest .
			docker tag chetanyagarg/flask_server:latest chetanyagarg/flask_server:${BUILD_NUMBER}
			
			'''}
		}

		stage("Push to Docker Hub") {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'docker-hub-creds',
            usernameVariable: 'DOCKER_USERNAME',
            passwordVariable: 'DOCKER_PASSWORD'
        )]) {
            sh '''
                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin

                docker push chetanyagarg/flask_server:latest
                docker push chetanyagarg/flask_server:${BUILD_NUMBER}

                docker logout
            '''
        }
    }
		}
		
		stage("deploy"){
			steps{sh ''' 
			docker rm -f flask_app || true
			
			docker run -d \
			-p 5005:5000 --name flask_app \
			chetanyagarg/flask_server:latest
			'''}
		}
	}


}
