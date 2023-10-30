#!groovy
pipeline{
	 environment { 
         DOCKERHUB_CREDENTIALS= credentials('dockerhub')     

    }

  agent {label "master"}
  
    stages
      {
        stage('Docker Build') 
         {
    	      steps 
           {
      	     print("building docker image") 
             sh "sudo chmod 777 /var/run/docker.sock"
             sh "docker build -t sanjayc09/2824"
		}
		}
		stage('Login') 
			{         
			steps{                            
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                 
				echo 'Login Completed'                
				}           
			} 
		
	 stage('Push') 
	   { 
            steps 
			{ 
			  sh "docker push sanjayc09/2824:latest"			
            } 
        }
         
     stage('create nodeport service')
        {
          steps {
            sh "sudo kubectl create -f k8s.yml"
          }
        }
    }
post {
	always{
		sh 'docker logout'
	}
}
}
