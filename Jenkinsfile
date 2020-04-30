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
	}
}