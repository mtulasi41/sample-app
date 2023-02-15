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
	stage('Integration Test'){
             steps{
	        script{
		        sh 'mvn verify -DskipUnitTests'
		}
	     }
	}
	stage('Maven Build'){
             steps{
	        script{
		        sh 'mvn clean install'
		}
	    }
	}
	stage('static code analysis'){
             steps{
	        script{
		        withSonarQubeEnv(credentialsId: 'sonar-app') {
   				 sh 'mvn clean package sonar:sonar'
		      }
		}
	    }
	}
	stage('Quality gate status'){
             steps{
	        script{
		        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-app'
		}
	    }
	}
    }
}
