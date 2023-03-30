pipeline {
  agent any
  tools { 
        maven 'maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=mytechapp -Dsonar.organization=mytechapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=6bc122a533d1d553ad932dce1ceb0ceb6ea211e9'
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
               withDockerRegistry([credentialsId: "c9a8863d-ce2b-48d5-b9d9-2ed2b68b7399", url: ""]) {
                 script{
                 app =  docker.build("tech365image")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
// 			583529678328.dkr.ecr.us-east-1.amazonaws.com/mytechimage
                    docker.withRegistry("https://583529678328.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:myawscredentials") 
			{
                    app.push("latest")
                    }
                }
            }
    	}


  }
}
