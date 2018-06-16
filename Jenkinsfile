pipeline {
        agent none 
    stages { 
                    
        
           stage('docker_build and push nginx') {agent any
            steps { 
		//sh 'docker rm  -f webserver'
		//sh 'docker-machine rm -y oRudenko'
                sh 'docker build -t grebec/test:${BUILD_NUMBER} .'
                sh 'docker tag grebec/test:${BUILD_NUMBER} grebec/test:latest'    
                withDockerRegistry([ credentialsId: "docker", url: ""]) 
                { sh 'docker push grebec/test:latest'}
            }
            }     
	   stage('Add conf, index push my_nginx') {agent any
            steps { 
                sh 'docker build -f Dockerfile1 -t grebec/test2:${BUILD_NUMBER} .'
                sh 'docker images'
                sh 'docker tag grebec/test2:${BUILD_NUMBER} grebec/test2:latest'    
                withDockerRegistry([ credentialsId: "docker", url: ""]) 
                { sh 'docker push grebec/test2:latest'}
            }
            }   
   	           stage('Deploy Nginx container') {agent any
            steps { 
		     withCredentials([[
            $class: 'AmazonWebServicesCredentialsBinding',
            credentialsId: 'aws',
            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
       				 ]]) 
		    {
               sh ('AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} AWS_DEFAULT_REGION=eu-west-2')
	       sh 'docker-machine create --driver amazonec2 --amazonec2-region eu-west-2  --amazonec2-zone a oRudenko'
	       sh 'docker-machine ls'
	       sh 'docker-machine env oRudenko'
	       sh 'eval $(docker-machine env oRudenko)'
	       sh 'docker run -d -p 80:80 --name webserver grebec/test2'		    
		    }
		    }
            }   
   }
}     
