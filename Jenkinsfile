pipeline {
    agent { label 'build_node' }
    stages {
        stage ('Git clone') {
            steps {
                git url: 'https://github.com/virutalneworg/openmrs-core.git', branch: 'master'
            }
        }
        stage('artifactory'){
            steps{
                rtMavenDeployer (
                    id: "openmrs_dep" ,
                    serverId: 'jfrog_instance',
                    releaseRepo: 'libs-release-local',
                    snapshotRepo: 'libs-snapshot-local',
                    deployArtifacts: 'true' 
                )
            }
        }
        stage ('build') {
            steps {
                withSonarQubeEnv('SONAR_SELF_HOSTED'){
                    sh 'mvn install sonar:sonar'
                }
            }
        }
		steps {
                // Use the rtMavenRun step to deploy artifacts to Artifactory
                rtMavenRun(
                    tool: 'mvn_2',
                    deployerId: 'jfrog_instance',
                    pom: 'pom.xml',
                    goals: 'deploy'
                )
            }
        stage('Publishtheartifacts'){
            steps{
                rtPublishBuildInfo (
                    serverId: 'jfrog_instance'
                )
            }
        }
    }
}
