pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/srinivas0256/Test.git"
            }
        }

        stage ('Artifactory Configuration') {
            steps {
                rtServer (
                    id: "Artifactory",
                    url: "http://localhost:8081/artifactory/",
                    credentialsId: "Jfrog"
                )

                rtMavenResolver (
                    id: 'maven-resolver',
                    serverId: 'Artifactory',
                    releaseRepo: libs-release-local,
                    snapshotRepo: libs-snapshot-local
                )  
                 
                rtMavenDeployer (
                    id: 'maven-deployer',
                    serverId: 'artifactory-server-id',
                    releaseRepo: ARTIFACTORY_LOCAL_RELEASE_REPO,
                    snapshotRepo: ARTIFACTORY_LOCAL_SNAPSHOT_REPO,
                    threads: 6,
                    properties: ['BinaryPurpose=Technical-BlogPost', 'Team=DevOps-Acceleration']
                )
            }
        }
        
        stage('Build Maven Project') {
            steps {
                rtMavenRun (
                    tool: 'maven',
                    pom: 'pom.xml',
                    goals: '-U clean install',
                    deployerId: "maven-deployer",
                    resolverId: "maven-resolver"
                )
            }
        }
		}
		}
