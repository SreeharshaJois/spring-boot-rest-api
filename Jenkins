pipeline {
	agent any

	environment {
		mavenHome = tool 'jenkins-maven'
	}

	tools {
		jdk 'java-17'
		maven 'jenkins-maven'
	}

	stages {

		stage('Build'){
			steps {
				sh "mvn clean install -DskipTests"
			}
		}
		 stage('SonarQube analysis') {
      			steps {
        			
        			withSonarQubeEnv('sonarqube') {
         			 	sh "mvn sonar:sonar"
        			}
			}
      		}
    
		stage('Test'){
			steps{
				sh "mvn test"
			}
		}

		stage('Deploy') {
			steps {
			    sh "mvn jar:jar deploy:deploy"
			}
		}
	}
}
