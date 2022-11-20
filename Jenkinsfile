// multistage
pipeline {
    agent any

        stages {
            stage('Source') {
                steps {
                    git url: 'https://github.com/Prakruthi0306/marketplace.git'
                }
            }
           
            
            stage ("Artifactory Publish"){
                steps{
                    script{
                            def server = Artifactory.server 'Artifactory'
                            def rtMaven = Artifactory.newMavenBuild()
                            //rtMaven.resolver server: server, releaseRepo: 'repo', snapshotRepo: 'snapshot'
                            rtMaven.deployer server: server, releaseRepo: 'repo', snapshotRepo: 'snapshot'
                            rtMaven.tool = 'MAVEN_HOME'
                            
                            def buildInfo = rtMaven.run pom: '$workspace/pom.xml', goals: 'clean install'
                            rtMaven.deployer.deployArtifacts = true
                            rtMaven.deployer.deployArtifacts buildInfo

                            server.publishBuildInfo buildInfo
                    }
                }
        }
        }
}
