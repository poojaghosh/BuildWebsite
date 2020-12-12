def dockerRun = "sudo docker run -it -p 85:80 -d --name VirtualCabinetry15CICD chika1984/myapp:15.0.0"
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
			sh 'docker build -t chika1984/myapp:15.0.0 .'

			}
		}	
		
		stage('Push Docker image') {
		agent { label 'master' }
		 steps {
			withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
            sh "docker login -u chika1984 -p ${dockerHubPwd}"
			}
			sh 'docker push chika1984/myapp:15.0.0'
		} 	
		}
		 stage('Run Docker image on STAGE Server') {
		 steps {
		    sshagent(['Prod-ADM-VC']){ 
			sh "ssh -o StrictHostKeyChecking=no ubuntu@13.232.83.228 ${dockerRun}"		 
		 }
        
}
}
}
}