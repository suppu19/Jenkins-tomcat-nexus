node {
    stage('test') {
        echo 'Knowledge Is Free'
    }
        echo "=======================================Checking Out Code from Github======================================="
       
        stage('Git Checkout') {
       
        git branch: 'main', credentialsId: 'Github', url: 'https://github.com/suppu19/project1.git'
       
        echo "=======================================Pulled Code from Github======================================="   
    }
   
    echo "=========================================Building Maven Project=========================================="
   
    stage('Maven Build') {
       
            sh "mvn clean package"
            
    }    
    stage("Archiving Artifact") {        
       
    echo "==========================================Built Maven Project============================================"
   
    echo "=========================================Archiving Artifact============================================="


        archiveArtifacts artifacts: '**/*.war'
   
    echo "==========================================Artifact Archieved============================================="
       
    }

stage('Deployment') {
sshagent(['tomcat-server']) {
sh "scp -o StrictHostKeyChecking=no $WORKSPACE/target/maven-web-application.war ubuntu@65.0.168.145:/opt/tomcat/webapps"
      }
    
}
       
stage('Nexus') {
    
    
    nexusArtifactUploader artifacts: [
        [
            artifactId: 'maven-web-application',
            classifier: '',
            file: 'target/maven-web-application.war',
            type: 'war'
        
        ]
], 
        credentialsId: 'nexus3',
        groupId: 'com.mt', 
        nexusUrl: '13.232.6.182:8081',
        nexusVersion: 'nexus3',
        protocol: 'http',
        repository: 'upload-artifact',
        version: '5.0.0'
    
    }
}
