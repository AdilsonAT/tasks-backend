pipeline {
	agent any
	
	stages {
		stage ('Build Backend') {
			steps {
				bat 'mvn clean package'
			}
		}
		
		stage ('Unit Tests') {
			steps {
				bat 'mvn test'
			}
		}

		stage ('Deploy Backend') {
			steps {
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: '/tasks-backend', war: 'target/tasks-backend.war'
     		}
     	}
     	
     	stage ('API Test') {
 			steps {
 				dir('api-test'){
 		           	git credentialsId: 'github_loggin', url: 'https://github.com/AdilsonAT/tasks-api-test' 
					bat 'mvn test'
				}
			}              
     	}
     	
 		stage ('Deploy Frontend') {
			steps {
				dir('frontend') {
					git credentialsId: 'github_loggin', url: 'https://github.com/AdilsonAT/tasks-frontend'
					bat 'mvn clean package'
					deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: '/tasks', war: 'target/tasks.war'
     			}
     		}
     	}
     	     	
     	stage ('Functional Test') {
 			steps {
 				dir('functional-test'){
 		           	git credentialsId: 'github_loggin', url: 'https://github.com/AdilsonAT/taskes-functional-test' 
					bat 'mvn test'
				}
			}              
    	}

     	stage ('Health Check') {
 			steps {
				sleep(5)
 				dir('functional-test'){ 
					bat 'mvn verify -Dskip.surefire.tests'
				}
			}              
    	}
    }

	post {
	    always {
	  		junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml, api-test/target/surefire-reports/*.xml, functional-test/target/surefile-reports/*.xml, functional-test/target/failsafe-reports/*.xml'
	    }
		unsuccessful{			    
			emailext attachLog: true, body: 'See the attached log bellow', subject: 'Build has failed', to: 'adilson.anjos.teixeira@gmail.com'
		}
		fixed{
		    emailext attachLog: true, body: 'See the attached log bellow', subject: 'Build is fine!!', to: 'adilson.anjos.teixeira@gmail.com'
		}
	}
}