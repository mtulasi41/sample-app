pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mtulasi41/sample-app.git']])
                }
            }
        }
       stage('Unit Testing'){
	     
	        steps{

		        script{
			
			        sh 'mvn test'
		        }
            }
        }
    }
}
