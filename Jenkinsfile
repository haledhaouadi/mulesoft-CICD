pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
    }
    environment {
        // Set environment variables for Splunk HEC
        SPLUNK_HEC_URL = 'http://localhost:8000'
        SPLUNK_HEC_TOKEN = 'ab2d52f7-3947-4527-a411-33f17c64143b'
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


