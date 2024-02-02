pipeline {
    
	agent any
/*	
	tools {
        maven "MAVEN3"
    }
*/	
    environment {
        NEXUS_VERSION = "nexus"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "172.31.90.111:8081"
        NEXUS_REPOSITORY = "santhu"
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        ARTVERSION = "${env.BUILD_ID}"
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

	stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

	stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
		
        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }



        stage('UploadArtifactsIntoNexus') {
	     steps {
                 sh  "mvn clean deploy -DrepositoryId=santhu"
	     }
        }
	    
                    
            
    }
}
