pipeline {
    agent any
    
    stages {
      
	  stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }    
	    
	     stage ('Check-Git-Secrets') {
		    steps {
	        
		sh ' docker pull gesellix/trufflehog'
		sh ' docker run -t gesellix/trufflehog --json https://github.com/natam369/webapp.git > trufflehog'
		sh 'cat trufflehog'
	    }
	    }
	    
	   
	   
	    
	
    }
}
