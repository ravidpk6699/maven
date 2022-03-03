pipeline {
    agent any
  tools {
    maven 'maven3'
	}
	stages{
	    stage('Build') {
		  steps{
		      sh 'mvn clean package'
      }
      }
	    stage('sonarqube analysis') {
		     steps{
			      withSonarQubeEnv('sonarqube') {
				  sh "mvn sonar:sonar"
				  }
			}
		}
		stage("Quality Gate") {
		steps {
		//timeout(time:1, unit: 'SECONDS') {
		//waitForQualityGate abortPipeline: true
		//}
		        echo 'test'
		}
	  }
	  stage('upload war to nexus'){
          steps{
	  nexusArtifactUploader artifacts: [[artifactId: 'WebApp', classifier: '', file: 'target/WebApp.war', type: 'war']], credentialsId: 'nexus3', groupId: 'Demoapp', nexusUrl: '20.212.18.14:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'projectoss-release', version: '1.1.1'
	  }
	  }
	  stage('deploy to tomcat'){
          steps{
		  sshagent(['tomcat']) {
            sh "scp -o StrictHostKeyChecking=no maven-app/target/WebApp.war azureuser@20.124.102.123:/opt/tomcat/webapps"
}
		  
	  } 
	  }
	  }
	  }
	     
	

	  
	  
	  
	  
