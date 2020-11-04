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
       git credentialsId: 'test_freestyleproject', url: 'https://github.com/nareshdara/maven-web-application.git' 
    }
    stage('build the source code'){
        sh "${mvnHome}/bin/mvn clean package"
    }
    stage('code quality test'){
        sh "${mvnHome}/bin/mvn clean sonar:sonar"
    }
    stage('store the artifact into nexus repository'){
        sh "${mvnHome}/bin/mvn clean deploy"
    }
    stage('deploy app into tomcat server'){
        sshagent(['test1_pipelineproject']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.141.76:/opt/apache-tomcat-8.5.59/webapps/maven-web-application.war"
}
    }
    stage('Email Notification'){
        emailext body: '''This is a pipeline test project

Thanks
Best Regards,
Naresh
''', subject: 'This is a pipeline test project', to: 'nareshdara200@gmail.com'
    }
    
    
}
 
