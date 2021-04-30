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
		stage('Upload Artifact to Jfrog') {
			steps {
				echo"Publishing to Artifactory"
				def server = Artifactory.server "SERVER_ID"
				 rtUpload (
                    buildName: JOB_NAME,
                    buildNumber: BUILD_NUMBER,
                    serverId: SERVER_ID, // Obtain an Artifactory server instance, defined in Jenkins --> Manage:
                    spec: '''{
                              "files": [
                                 { "pattern": "**/target/*.jar",
								   "target": "libs-snapshot-local"
                                   "recursive": "false"
                                } 
                             ]
                        }'''    
                    )
	  
	  
	 }
	 }
	 }
	 }
		
