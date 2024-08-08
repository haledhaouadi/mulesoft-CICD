pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
    }
    environment {
        // Set environment variables for Splunk HEC
        SPLUNK_HEC_URL = 'http://localhost:8088' // Corrected port to 8088
        SPLUNK_HEC_TOKEN = '70e79e4f-82ee-4632-b8bd-6188e2d1bdab'
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
                        
                        // Send logs to Splunk, with properly escaped JSON
                        sh """
                        curl -k "${env.SPLUNK_HEC_URL}/services/collector" \
                        -H "Authorization: Splunk ${env.SPLUNK_HEC_TOKEN}" \
                        -d '{"event": "Build logs: ${buildOutput.replaceAll('"', '\\"')}" }'
                        """
                    }
                }
            }
        }
    }
}
