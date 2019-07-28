node{
    def mvnHome = tool name: 'maven3.6.1' , type: 'maven'
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
                  
                  ])
                  
    stage('CheckoutCode'){
     git credentialsId: 'f7caf0c9-19e6-4a7b-a678-126b1cf5d9ad', url: 'https://github.com/nareshdara/maven-web-application.git'   
    }
    stage('Buildthe Code'){
        sh "${mvnHome}/bin/mvn clean package"
        
     
    }
    
/*
    stage('ExecuteSonarqubeReport'){
        sh "${mvnHome}/bin/mvn sonar:sonar"
    }
    stage('store ArtifactintoNexus'){
        sh "${mvnHome}/bin/mvn deploy"
        
    }
   
    stage('DeployAppIntoTomcatServer'){
        sshagent(['tomcatserver']) {
       sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@http://13.233.17.201/opt/apache-tomcat-9.0.20/webapps/maven-web-application.war"
}
        
    }
   */
  
}
