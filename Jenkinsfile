pipeline{
  agent any
	stages('Git Checkout'){
		steps {
			checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mtulasi41/sample-app.git']])
	    }
    }
}
