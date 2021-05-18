node{
   
    def mvnHome = tool name: 'maven' , type: 'maven'
   /*
    echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
   
    properties([
                buildDiscarder(logRotator(numToKeepStr: '3')),
                pipelineTriggers([
                    pollSCM('* * * * *')
                    ])
                  
                  ])  */
                  
  
  stage("check out the source code"){
        git credentialsId: 'githubcredentials', url: 'https://github.com/nareshdara/maven-web-application.git'
    }
    stage("build the source code"){
        sh "${mvnHome}/bin/mvn clean package"
    }
    stage("check out the code quality test"){
        sh "${mvnHome}/bin/mvn sonar:sonar"
    }
    stage("store the artifact into nexus repository"){
        sh "${mvnHome}/bin/mvn clean deploy"
    }
    stage("deploy application into tomcat server"){
        sshagent(['tomcat_sshagent_credentials']) {
       sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.119.160:/opt/apache-tomcat-9.0.46/webapps/maven-web-application.war" 
}
        
    }
    stage("email notification"){
        emailext body: 'email text message', subject: 'email text message', to: 'nareshdara200@gmail.com'
        
    }
    
}
