node {

def mavenHome = tool name: 'maven3.9.5'
echo "The job name is: ${env.JOB_NAME}"
echo "Jenkins home directory: ${env.JENKINS_HOME}"
echo "The jenkins node name is: ${env.NODE_NAME}"
echo "The build number is: ${env.BUILD_NUMBER}"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], pipelineTriggers([pollSCM('* * * * *')])])

stage('checkoutcode'){
git branch: 'main', credentialsId: 'c0e9e806-2b0f-495f-9291-34915796e466', url: 'https://github.com/mabasha-practice/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
stage('EXECUTEsonarqubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}
stage('upload artifact into nexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('deploy app into tomcat server'){
sshagent(['f6d26bdb-9bbd-46eb-b899-e499936816de']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.34.232:/opt/apache-tomcat-9.0.95/webapps/"
}
}
}
