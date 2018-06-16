pipeline {
        agent none 
    stages { 
          stage ('create inst') { agent any
              steps {
                withCredentials([[
            $class: 'AmazonWebServicesCredentialsBinding',
            credentialsId: 'adc00aa9-73ed-456f-bcd1-1a8cfdaba58b',
            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ]]) 
                      {
            sh ('> hosts ')            
            sh ('AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} AWS_DEFAULT_REGION=us-west-2')
            //sh ('AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} AWS_DEFAULT_REGION=us-west-2 ${AWS_BIN}')
	    //sh('/home/ubuntu/print.sh')
                        withAWS(region:'us-west-2'){
                                sh('python3.5 start.py')} 
                        //echo 'Waiting deployment to complete start inst'
                        //sleep 200 // seconds
			}
               }
           }
          
       
            stage('docker_build and push nginx') {agent any
            steps { 
                sh 'docker build -t grebec/test:${BUILD_NUMBER} .'
                sh 'docker images'
                sh 'docker tag grebec/test:${BUILD_NUMBER} grebec/test:latest'    
                withDockerRegistry([ credentialsId: "ad5a78f7-c1af-4b37-a58f-ae20d9244457", url: ""]) 
                { sh 'docker push grebec/test:latest'}
            }
            }    
	           stage('Add conf, index push my_nginx') {agent any
            steps { 
                sh 'docker build -t grebec/test2:${BUILD_NUMBER} .'
                sh 'docker images'
                sh 'docker tag grebec/test2:${BUILD_NUMBER} grebec/test:latest'    
                withDockerRegistry([ credentialsId: "ad5a78f7-c1af-4b37-a58f-ae20d9244457", url: ""]) 
                { sh 'docker push grebec/test2:latest'}
            }
            }   
   	         /*  stage('Deploy Nginx container') {agent any
            steps { 
               
            }
            }  */ 
   }
}     
