 node('master'){
 def mavenHome = tool name: "Maven3.8.4"
 
  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
 
 stage('CheckoutCode'){
 git branch: 'development', credentialsId: 'e007c237-602f-4524-beba-a09de7b7f92f', url: 'https://github.com/devops-hira/maven-web-application.git'
  }
 stage('Build'){
 sh "${mavenHome}/bin/mvn clean package"
 }
 stage('SonarQubeReport'){
 sh "${mavenHome}/bin/mvn sonar:sonar"
 }
  stage('UploadArtifactsIntoNexus'){
 sh "${mavenHome}/bin/mvn deploy"
 }
 stage('DeployAppintoOCITomcatServer'){
  sshagent(['e15105c3-f97d-46b6-bbc1-7fd4ec6b6cef']) {
       // some block
	sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war opc@152.70.240.65:/opt/apache-tomcat-9.0.55/webapps/"   
   }
 }
 stage('sendemailNotification'){
 emailext body: '''Build Over

 Regards
 Hiralal Sahu''', subject: 'Build Over', to: 'hiralal.sahu@gmail.com'
  }
   
 }
