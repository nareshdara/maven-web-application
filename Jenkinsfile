node{
   
    def mvnHome = tool name: 'maven_3.6.3' , type: 'maven'
   /*
    echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
    */
    properties([
                buildDiscarder(logRotator(numToKeepStr: '3')),
                pipelineTriggers([
                    pollSCM('* * * * *')
                    ])
                  
                  ])
                  
  
    stage('checkout the source code'){
        git credentialsId: 'test_maven_freestyle_project', url: 'https://github.com/nareshdara/maven-web-application.git'
     }
    stage('build the source code'){
        sh "${mvnHome}/bin/mvn clean package"
    }
    stage('code quality check'){
        sh "${mvnHome}/bin/mvn sonar:sonar"
    }
    stage('store the artificat into nexus repository'){
        sh "${mvnHome}/bin/mvn deploy"
    }
    stage('deploy webapp into tomcat server'){
        
        sshagent(['test_maven_sshagent']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.203.3:/opt/apache-tomcat-9.0.40/webapps/maven-web-application.war"
}
        
    }
    stage('send EmailNotification'){
        
        emailext body: '''this is for test message

regards,
Naresh''', subject: 'this is for test message', to: 'nareshdara200@gmail.com'
    }
}
