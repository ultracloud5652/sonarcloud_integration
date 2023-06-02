pipeline {
  agent any
  tools { 
        maven 'maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=ultracloud -Dsonar.organization=ultracloud -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=aa2c5ace82e378c9eac4f76e9109b33fd1330d63'
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
                 app =  docker.build("ultraimage")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry("https://769993731789.dkr.ecr.us-east-1.amazonaws.com/ultraimage", "ecr:us-east-1:myawscredentials") 
			{
                    app.push("latest")
                    }
                }
            }
    	}

      // Kubernetes
  stage('Kubernetes Deployment of Easy Buggy Web Application') {
	   steps {
	      withKubeConfig([credentialsId: 'kubelogin']) {
		  sh('kubectl delete all --all -n devsecops')
		  sh ('kubectl apply -f deployment.yaml --namespace=devsecops')
		}
	      }
   	}

   
	    
  }
}

