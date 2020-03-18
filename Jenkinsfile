
@Library(['pipeline-ao-libs', 'pipeline-common-libs@hotfix/DEVENA-1653']) _

node {
    stage('Checkout SCM') {
        utility.gitCheckout("${env.WORKSPACE}", true)
    }
    stage('Download Java and Maven') {
        dockerBuildTools("java", "2019.07.29.12.24.59", 8)
    }
    stage('Download Maven dependencies'){
        sh script: "${env.WORKSPACE}/.tools/maven/bin/mvn -s ${env.JENKINS_HOME}/settings.xml -Dmaven.repo.local=${env.JENKINS_HOME}/.m2/repo-${env.EXECUTOR_NUMBER} clean install"
    }
    stage('Scan dependencies with Black Duck'){
        runBlackDuckJava {
            logLevel = 'TRACE'
        }
    }
}
