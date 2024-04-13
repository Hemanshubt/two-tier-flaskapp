pipeline{
	agent any
	
	stages{
		stage("Code"){
			steps{
			    git url:"https://github.com/sanatkp84/two-tier-flaskapp.git", branch:"main"
			}
		}
		stage("Build & Test"){
			steps{
			    sh "docker build . -t docker-todo-app"
			}
		}
		stage("Push to Repo"){
			steps{
			    withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag docker-todo-app ${env.dockerHubUser}/docker-todo-app:latest"
                    sh "docker push ${env.dockerHubUser}/docker-todo-app:latest" 
                }
			}
		}
		stage("Deploy"){
			steps{
			    sh "docker-compose down && docker-compose up -d"
			}
		}
	}
}

