node{
   stage('SCM Checkout'){
     git 'https://github.com/vedantek/java-maven-war'
   }
   stage('Compile-Package'){
      // Get maven home path
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
   }
   stage('Slack Notification'){
       slackSend baseUrl: 'https://hooks.slack.com/services/',
       channel: '#cicd-b01',
       color: 'good', 
       message: 'Jenkins Build Success!', 
       teamDomain: 'vedantek',
       tokenCredentialId: 'slack'
   }
   stage('Upload to S3'){
   s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'cloud-front-through-s3-bucket', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: 'target/*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'jayeshaws', userMetadata: []
   }
   
   stage('Deploy to Tomcat'){
   sshagent(['tomcat-server']) {
    sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/java-war-cicd/target/myweb-0.0.1.war ubuntu@3.93.144.30:/var/lib/tomcat9/webapps/'
   }
   }
   stage('Slack Notification'){
       slackSend baseUrl: 'https://hooks.slack.com/services/',
       channel: '#cicd-b01',
       color: 'good', 
       message: 'Package is uploaded to S3!', 
       teamDomain: 'vedantek',
       tokenCredentialId: 'slack'
   }
}
