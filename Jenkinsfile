pipeline {
  agent any
  tools { 
        maven 'maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=techapp365 -Dsonar.organization=techapp365 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=045f1be9d414a138acdaa2eeb4c79a7ffefd1ebc'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }	

// building docker image
 stage('Build') { 
           steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("follyimage")
                 }
               }
            }
    }

stage('Push') {
            steps {
                script{
                    docker.withRegistry("https://813817168689.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:myawscredentials") 
			{
                    app.push("latest")
                    }
                }
            }
    	}
	

  }
}
