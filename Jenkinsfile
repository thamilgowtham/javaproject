pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git url:'https://github.com/thamilgowtham/javaproject.git', branch: 'main'
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Generate sonarqube-analysis'){
            steps{
                withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonar-token') {
                sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Upload jar file to Nexus'){
            steps{
             nexusArtifactUploader artifacts: [
  	    	[
   	  	    artifactId: 'javaproject',
   		    classifier: '', 
   		    file: 'target/javaproject-0.0.1-SNAPSHOT.jar',
   		    type: 'jar'
  		    ]
	       ], 
	    credentialsId: 'nexus-credentials', 
	    groupId: 'com.example', 
	    nexusUrl: '34.219.223.174:8081', 
	    nexusVersion: 'nexus3', 
	    protocol: 'http', 
	    repository: 'newmavenrepo', 
	    version: '0.0.1-SNAPSHOT'
             }
        }
    }
}
