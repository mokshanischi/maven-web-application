node
{
    
def mavenHome = tool name: "maven 3.6.3"
  
  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), 
              pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode')
{
git branch: 'development', credentialsId: '841c5c96-8831-46e3-8a87-181059e5860b', 
url: 'https://github.com/mokshanischi/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
} 
stage('UploadartifactsintoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
} 
stage('DeploytheappintoTomcatserver')
{
sshagent(['cbaa2305-b887-4549-9f6d-db56419daa47']){
sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.207.221.55:/opt/apache-tomcat-9.0.41/webapps/"
}
}
}
