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
				maven 'apache-maven-3.5.4'
			}
			steps {
				powershell 'java -version'
				powershell 'mvn -version'
				powershell 'mvn clean package'

			}
		}
		stage('Deploy') {
			steps{
				echo "Deploying"
				deploy adapters: [tomcat7(credentialsId: '613b2e60-93b3-4524-a02d-b8d92e1d093e', path: '', url: 'http://localhost:8085')], contextPath: 'happytrip', war: '**/*.war'
			}
		}
	}
}
