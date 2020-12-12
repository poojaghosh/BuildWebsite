def dockerRun = "sudo docker run -it -p 91:80 -d --name WebsiteBuilderCICD chika1984/myapp:11.0.1"
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
			sh 'docker build -t chika1984/myapp:11.0.1 .'

			}
		}	
		
		stage('Push Docker image') {
		agent { label 'master' }
		 steps {
			withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
            sh "docker login -u chika1984 -p ${dockerHubPwd}"
			}
			sh 'docker push chika1984/myapp:11.0.1'
		} 	
		}
		 stage('Run Docker image on STAGE Server') {
		 steps {
		    sshagent(['Staging-ADM-VC']){ 
			sh "ssh -o StrictHostKeyChecking=no ubuntu@13.233.132.33 ${dockerRun}"		 
		 }
        
}
}
}
}