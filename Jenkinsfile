pipeline {
    agent any
    
    stages {
        
	    
	   

	  stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }    
	    
	
       stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@18.139.116.89:/home/ubuntu/prod/apache-tomcat-8.5.39/webapps/webapp.war'
              }      
           }       
    }
	     stage ('Check-Git-Secrets') {
		    steps {
	        sh 'rm trufflehog || true'
		sh 'docker pull gesellix/trufflehog'
		sh 'docker run -t gesellix/trufflehog --json https://github.com/natam369/webapp.git > trufflehog'
		sh 'cat trufflehog'
	    }
	    }
	    
	   
	    stage ('Upload Reports to Defect Dojo') {
		    steps {
			sh 'pip install requests'
			sh 'wget https://raw.githubusercontent.com/devopssecure/webapp/master/upload-results.py'
			sh 'chmod +x upload-results.py'
			sh 'python upload-results.py --host 3.81.3.77:80 --api_key 66879c160803596f132aff025fee9a170366f615 --engagement_id 4 --result_file trufflehog --username admin --scanner "SSL Labs Scan"'
			sh 'python upload-results.py --host 3.81.3.77:80 --api_key 66879c160803596f132aff025fee9a170366f615 --engagement_id 4 --result_file /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml --username admin --scanner "Dependency Check Scan"'
			sh 'python upload-results.py --host 3.81.3.77:80 --api_key 66879c160803596f132aff025fee9a170366f615 --engagement_id 4 --result_file nmap --username admin --scanner "Nmap Scan"'
			sh 'python upload-results.py --host 3.81.3.77:80 --api_key 66879c160803596f132aff025fee9a170366f615 --engagement_id 4 --result_file sslyze-output.json --username admin --scanner "SSL Labs Scan"'
			sh 'python upload-results.py --host 3.81.3.77:80 --api_key 66879c160803596f132aff025fee9a170366f615 --engagement_id 4 --result_file nikto-output.xml --username admin'
			    
		    }
	    }
	    
	
    }
}
