pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
    }
    environment {
        SPLUNK_HEC_URL = 'http://localhost:8088' // Corrected port to 8088
        SPLUNK_HEC_TOKEN = 'e6e678e9-29ae-4731-a960-4a0807a2e4a1'
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
                        echo buildOutput

                        // Properly formatted and escaped JSON payload
                        def jsonPayload = """{"event":"Build logs: ${buildOutput.replaceAll('"', '\"')}"}"""

                        sh(script: """
                        curl -k "${env.SPLUNK_HEC_URL}/services/collector" \
                        -H "Authorization: Splunk ${env.SPLUNK_HEC_TOKEN}" \
                        -d "${jsonPayload}"
                        """)
                    }
                }
            }
        }
    }
}
