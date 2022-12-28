pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=sonarappkey -Dsonar.organization=sonarappkey -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=f03db338e8cd5ae5637679af3b12e36e6a2b25e6'
			}
    }

// 	stage('RunSCAAnalysisUsingSnyk') {
//             steps {		
// 				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
// 					sh 'mvn snyk:test -fn'
// 				}
// 			}
//     }	

// building docker image
stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("tech365image")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry("https://252889606091.dkr.ecr.us-east-2.amazonaws.com", "ecr:us-east-1b:aws_credentials") 
			{
                    app.push("latest")
                    }
                }
            }
    	}


  }
}
