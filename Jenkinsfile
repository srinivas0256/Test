pipeline {
	
		agent any 
		def server = Artifactory.server "SERVER_ID"
		def rtMaven = Artifactory.newMavenBuild()
		def buildInfo
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
		stage('Artifactory configuration') {
        // Tool name from Jenkins configuration
        rtMaven.tool = "maven"
        // Set Artifactory repositories for dependencies resolution and artifacts deployment.
        rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    }

    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'Test'/pom.xml', goals: 'clean install'
    }

    stage('Publish build info') {
        server.publishBuildInfo buildInfo
    }
	}
	}
