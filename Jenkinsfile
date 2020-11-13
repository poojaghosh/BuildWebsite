def dockerRun = "sudo docker run -it -p 8085:80 -d --name WebsiteCItoCD chika1984/myapp:9.0.0"
pipeline {
  agent any
      stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
					echo 'this **********'
                    '''
            }
        }
    
      stage('Build Docker image') {
		  agent { label 'master' }
		  	  
         steps {
			sh 'docker build -t chika1984/myapp:9.0.0 .'

			}
		}	
		
		stage('Push Docker image') {
		agent { label 'master' }
		 steps {
			withCredentials([string(credentialsId: 'Docker-Hub-Pwd-Main', variable: 'dockerHub')]) {
            sh "docker login -u chika1984 -p ${dockerHub}"
			}
			sh 'docker push chika1984/myapp:9.0.0'
		} 	
		}
		stage('Deleting any existing Docker container') {
		  agent { label 'Staging-Hack' }
		  steps {
			sh 'sudo docker rm -f $(sudo docker ps -a -q)'

			}
		}
		 stage('Run Docker image on STAGE Server') {
		 steps {
		    sshagent(['Staging-Hack']){ 
			sh "ssh -o StrictHostKeyChecking=no ubuntu@15.206.89.92 ${dockerRun}"		 
		 }
        
}
}
}
}