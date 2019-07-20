node('master'){
    def mvnhome = tool name: 'maven3.6.1', type: 'maven'
    //def mvnHome = tool name: 'maven3.6.1', type: 'maven'
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3'))])
     //properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3'))])
    stage('CheckoutCode'){
      git credentialsId: '134795ec-ad1f-4983-8eb6-4b06b26f1ff7', url: 'https://github.com/nareshdara/maven-web-application.git'  
        }
        
    stage('build'){
        sh "${mvnhome}/bin/mvn clean package"
        }    
    stage('Generate a Report'){
        sh "${mvnhome}/bin/mvn sonar:sonar"
    }    
    stage('Store Artifact into Nexus'){
        sh "${mvnhome}/bin/mvn deploy"
    }
    stage('Deploy into Tomcat Server'){
        sshagent(['TomcatServer']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.86.80:/opt/apache-tomcat-9.0.22/webapps/maven-web-application.war"
   // sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.166.193:/opt/apache-tomcat-9.0.22/webapps/maven-web-application.war"
}
    }
    
}
