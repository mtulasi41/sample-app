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
			def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "sampleapp-snapshot" : "sampleapp-release"
		       nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']],
			    				credentialsId: 'nexus', 
			       				groupId: 'com.example', 
			       				nexusUrl: '3.109.213.35:8081', 
			       				nexusVersion: 'nexus3', 
			       				protocol: 'http', 
			       				repository: nexusRepo, 
			       				version: "${readPomVersion.version}"
		}
	    }
	}
	stage('Docker Image Build'){
             steps{
	        script{
		        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
			sh 'docker image tag $JOB_NAME:v1.$BUILD_ID chtulasi/$JOB_NAME:v1.$BUILD_ID'
			sh 'docker image tag $JOB_NAME:v1.$BUILD_ID chtulasi/$JOB_NAME:v1.latest'
		}
	     }
	}
	stage('Push Image to dockerhub'){
             steps{
	        script{
		        withCredentials([string(credentialsId: 'git-cred', variable: 'docker_hub_cred')]) {
				sh 'docker login -u chtulasi -p${docker_hub_cred}'
				sh 'docker image push chtulasi/$JOB_NAME:v1.$BUILD_ID'
				sh 'docker image push chtulasi/$JOB_NAME:v1.latest'
			}
		  }
	     }
	}
    }
}
