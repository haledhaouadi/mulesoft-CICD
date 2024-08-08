pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
    }
    environment {
        // Set environment variables for Splunk HEC
        SPLUNK_HEC_URL = 'http://<splunk-server>:8088'
        SPLUNK_HEC_TOKEN = 'YOUR_SPLUNK_HEC_TOKEN'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                dir('project') { // Navigate to the 'project' directory
                    script {
                        def buildOutput = sh(script: "${tool 'maven'}/bin/mvn clean package -e -X", returnStdout: true).trim()
                        // Print the output to the Jenkins console
                        echo buildOutput
                        
                        // Send logs to Splunk
                        sh """
                        curl -k "${env.SPLUNK_HEC_URL}/services/collector" \
                        -H "Authorization: Splunk ${env.SPLUNK_HEC_TOKEN}" \
                        -d '{"event": "Build logs: ${buildOutput}"}'
                        """
                    }
                }
            }
        }
    }
}


