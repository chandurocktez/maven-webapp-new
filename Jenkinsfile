node(){
    def mvnHome = tool name:'Maven3.6.1',type:'maven'
    
     properties([
       buildDiscarder(logRotator(numToKeepStr: '5')),
       pipelineTriggers([
           pollSCM('* * * * *')
       ])
   ])
    stage('Checkout from Github'){
        git credentialsId: '4c10f30d-f551-412a-a2f6-2a23689b7d15', url: 'https://github.com/chandurocktez/maven-webapp-new.git'
    }
    stage('Creating a Build'){
        if(isUnix()){
            sh "${mvnHome}/bin/mvn clean package"
        }
        else{
            bat "${mvnHome}/bin/mvn clean package"
        }
    }
    stage('Generating Sonarqube Reports'){
        if(isUnix()){
            sh "${mvnHome}/bin/mvn clean sonar:sonar"
        }
        else{
            bat "${mvnHome}/bin/mvn clean sonar:sonar"
        }
    }
    stage('Storing Artifacts into Nexus'){
        if(isUnix()){
            sh "${mvnHome}/bin/mvn clean deploy"
        }
        else{
            bat "${mvnHome}/bin/mvn clean deploy"
        }
    }
    stage("Deploying into Tomcat"){
        sshagent(['fef701bc-40c5-43e3-9500-6c3d22048df2']) {
    // some block
    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@13.127.188.118:/opt/apache-tomcat-9.0.21/webapps/maven-web-application.war"
}
    }
}
