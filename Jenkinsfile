node{
   
    def mvnHome = tool name: 'maven-3.6.3' , type: 'maven'
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
                  
  
    stage('checkout the code'){
        git credentialsId: 'mavenwebapp_git_credentials', url: 'https://github.com/nareshdara/maven-web-application.git'
        
    }
    stage('build the source code'){
        sh "${mvnHome}/bin/mvn clean package"
        
    }
    stage('get the source code'){
        sh "${mvnHome}/bin/mvn sonar:sonar"
    }
    stage('store the artifact into nexus repository'){
        sh "${mvnHome}/bin/mvn deploy"
        
    }
    stage('deploy app into tomcat server'){
        sshagent(['mavenwebapp_tomcat_credentials']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.114.184:/opt/apache-tomcat-9.0.36/webapps/maven-web-application.war"
}
        
    }
    stage('send an email notification'){
        emailext body: '''Regards,
Naresh''', subject: 'mavenwebapp', to: 'nareshdara200@gmail.com'
        
    }
    
    
}
 
