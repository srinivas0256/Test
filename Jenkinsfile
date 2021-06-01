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
		 stage ('Publishing Artifacts to Jfrog') {
            steps {
                rtServer (
                    id: "Artifactory",
                    url: "http://localhost:8081/artifactory",
                    credentialsId: "Artifactory"
                )
				bat 'mvn clean package deploy -U -DskipMunitTests'
			}
		}
		
		    stage('Deploy CloudHub') {
				steps {
    
					step{credentialsId: ('anypoint.credentails')
		url: 'https://anypoint.mulesoft.com/cloudhub/'
       // muleEnv = "${env.cloudhub_env.toLowerCase()}"
					    }
     
		echo "----Deploy To CloudHub----- "
        echo "----Running Build ${env.BUILD_ID} on muleEnv - dev----- "
        bat 'mvn clean package deploy -DskipMunitTests -DmuleDeploy -P' 
      
    }
		
	
}
}
}
