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
                    id: "MAVEN_DEPLOYER" ,
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
        stage('Publishtheartifacts'){
            steps{
                rtPublishBuildInfo (
                    serverId: 'jfrog_instance'
                )
            }
        }
    }
}
