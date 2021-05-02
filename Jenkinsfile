pipeline {
	
		agent any 
		tools {
		jdk 'jdk'
		maven 'maven'
		}
	stages {
	
		stage ('Initialize') {
            steps {
                bat '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
       }
	   }
		stage('Git Checkout') {
			steps{
				git credentialsId: 'Git', 
				url: 'https://github.com/srinivas0256/Test.git'
				}
		}
		
		stage('Unit Test') {
			steps {
				echo "Munit Test Checking Code Coverage"
					bat 'mvn clean test'
				}
			}
		 stage ('Artifactory Configuration') {
            steps {
                rtServer (
                    id: "Artifactory",
                    url: "http://localhost:8081/artifactory",
                    credentialsId: "Jfrog"
					
			
                )
			}
		}
		stage('Upload to Jfrog') {
			steps {
				rtUpload (
					id: "Artifactory",
                    spec: '''{
						"files": [
					{
						"pattern": "**/target/*.jar",
						"target": "libs-release/"
            }
         ]
    }'''
					
				
	}
}
}
}
		
