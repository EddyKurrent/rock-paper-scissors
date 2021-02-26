node {
    def server = Artifactory.server 'mill-artifactory+xray'
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo

    stage ('Clone') {
        git url: 'https://github.com/EddyKurrent/rock-paper-scissors.git'
    }

    stage ('Artifactory configuration') {
        rtMaven.tool = 'maven_3_6_3' 
        rtMaven.deployer releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Exec Maven') {
        rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
