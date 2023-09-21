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

		stage('Upload Artefact to Nexus Repository') {
			steps {
			    nexusArtifactUploader(
				        nexusVersion: 'nexus3',
				        protocol: 'http',
				        nexusUrl: 'http://localhost:8081',
				        groupId: 'id.test',
				        version: '1.0.0',
				        repository: 'springboot-app-repo',
				        credentialsId: 'nexusCredentials',
				        artifacts: [
				            [artifactId: "spring-boot-testing",
				             classifier: '',
				             file: 'spring-boot-testing-' + '1.0.0.jar',
				             type: 'jar']
				        ]
     				)
			}
		}
	}
}
