pipeline{
    agent any 
	tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Mven3.9"
    }
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
