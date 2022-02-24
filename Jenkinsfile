pipeline{
    agent any
    options {
      timeout(30)
    }
    stages{
        
        stage('Maven and Sonar'){
            
            parallel{
            stage('Sonar Analysis'){
                steps{
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn sonar:sonar'
                    }
                    
                    timeout(time: 1, unit: 'HOURS') {
                        script{
                          def qg = waitForQualityGate()
                          if (qg.status != 'OK') {
                              error "Pipeline aborted due to quality gate failure: ${qg.status}"
                          }
                        }
                  }
                }
            }
            
             stage('mvn Build'){
                steps{
                    sh 'mvn clean package'
                }
            }
        
     
        }
        
        }
        stage('upload war to nexus'){
          steps{
	  nexusArtifactUploader artifacts: [[artifactId: 'WebApp', classifier: '', file: 'target/WebApp.war', type: 'war']], 
	  credentialsId: 'nexus3',
	  groupId: 'Demoapp',
	  nexusUrl: '20.212.18.14:8081',
	  nexusVersion: 'nexus3',
	  protocol: 'http', repository: 'projectoss-release',
	  version: '1.1.1'
            }
        }
          
        stage('Tomcat Deploy'){
            steps{
                tomcatDeploy("20.24.38.22","azureuser","WebApp")
            }
        }
     
    }
}

