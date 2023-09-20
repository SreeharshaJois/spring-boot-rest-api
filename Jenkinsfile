pipeline {
	agent any

	environment {
		mavenHome = tool 'jenkins-maven'
	}

	tools {
		jdk 'java-11'
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

        stage('OWASP Dependency-Check Vulnerabilities') {
              steps {
                dependencyCheck additionalArguments: '''
                            -o './'
                            -s './'
                            -f 'ALL'
                            --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
              }
        }

		stage('Deploy') {
			steps {
			    sh "mvn jar:jar deploy:deploy"
			}
		}
	}
}
