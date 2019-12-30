pipeline {
	agent any
	stages {
		stage('Source') { 
			steps {
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/chanchalsoni123/discoveri-happytrip.git']]])
			}
		}
		stage('Build') { 
			tools {
				jdk 'jdk8'
				maven 'apache-maven-3.6.1'
			}
			steps {
				//powershell 'java -version'
				//powershell 'mvn -version'
				powershell 'mvn clean package'

			}
		}
		
		
		post {
			stage('Notification')
			{
       
      success {  
            mail bcc: '', body: "<b>Build Status - success</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "chnchlsoni@gmail.com";
      }  
        failure {  
          mail bcc: '', body: "<b>Build Status - Failed</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "chnchlsoni@gmail.com";  
        }  
			}
    }  
		
		
		stage ('Archive')
		{
			steps {
			  
			  archiveArtifacts '**/*.jar'
			
			}
		   
		
		}
		
		
		stage('Approval') {
            // no agent, so executors are not used up when waiting for approvals
            agent none
            steps {
                script {
                    //def deploymentDelay =
	           input id: 'Deploy', message: 'Deploy to production?', submitter: 'chanchal,admin', parameters: [choice(name: 'Approval', choices: ['YES', 'NO'], description: 'Deploy from STAGING to PRODUCTION?')]
                    
			//sleep time: deploymentDelay.toInteger(), unit: 'HOURS'
                }
            }
        }
		stage('Deploy') {
			steps{
				
		                echo "Deploying"
				deploy adapters: [tomcat7(credentialsId: '613b2e60-93b3-4524-a02d-b8d92e1d093e', path: '', url: 'http://localhost:8085')], contextPath: 'happytrip', war: '**/*.war'
			}
		}
	}
	
	
	
	
	triggers {
    pollSCM '* * * * *'
}
	
	
}
