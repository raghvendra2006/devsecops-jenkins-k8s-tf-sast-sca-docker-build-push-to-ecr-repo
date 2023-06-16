pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=buggywebapp002 -Dsonar.organization=buggywebapp002 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=59a7deca67268789983da4b436c23bc6e6517653'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://613266807092.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
