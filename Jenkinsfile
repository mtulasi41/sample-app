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
	stage('Upload Jar file to nexus'){
             steps{
	        script{
			def readPomVersion = readMavenPom file: 'pom.xml'
			def nexusRepo = readPomVersion.version.endswith("SNAPSHOT") ? "sampleapp-snapshot" : "sampleapp-release"
		       nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']],
			    				credentialsId: 'nexus', 
			       				groupId: 'com.example', 
			       				nexusUrl: '52.66.252.188:8081', 
			       				nexusVersion: 'nexus3', 
			       				protocol: 'http', 
			       				repository: nexusRepo, 
			       				version: "${readPomVersion.version}"
		}
	    }
	}
    }
}
