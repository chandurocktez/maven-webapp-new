node('node1'){
    def mvnHome = tool name:'Maven 3.6.1', type:'maven'
stage('Checkout the Code from Github'){
    git credentialsId: '12a4597f-4820-4862-820d-bc820c8c757a', url: 'https://github.com/chandurocktez/maven-webapp.git'
}
stage('Build the Package using Maven Build Tool'){
    sh "${mvnHome}/bin/mvn clean package"
}
stage('Generating Reports using Sonarqube'){
    sh "${mvnHome}/bin/mvn clean package sonar:sonar"
}
    stage('Storing the ARtifacts in Nexus Repository'){
        sh "${mvnHome}/bin/mvn clean deploy sonar:sonar"
    }
stage('Deploying the Package in to Tomcat Server'){
    sshagent(['Tomcat-Tez-Key1']) {
    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@13.127.146.128:/opt/apache-tomcat-9.0.21/webapps/maven-web-application.war"
}
}

}
