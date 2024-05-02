node{
    
def mavenHome = tool name: 'maven3.9.5'

echo "The Job name is : ${env.JOB_NAME}"
echo "Jenkins Home dir is: ${env.JENKINS_HOME}"
echo "THe Jenkins node name is: ${env.NODE_NAME}"
echo "The Build number is: ${env.BUILD_NUMBER}"


properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode'){
git branch: 'development', credentialsId: 'b5b3819d-335b-4faf-9b1f-ac6e7e1f172a', url: 'https://github.com/sss-banking-march7am/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat'){
sshagent(['b5b3819d-335b-4faf-9b1f-ac6e7e1f172a']) {
 sh "scp -o StrictHostKeyChecking=no  target/maven-web-application.war ec2-user@172.31.47.154:/opt/apache-tomcat-9.0.86/webapps/"
}
}


}
