pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
    }
    environment {
        SPLUNK_HEC_URL = 'http://192.168.1.20:8088'
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
                        // Send logs to Splunk
                        sendLogsToSplunk(buildOutput)
                    }
                }
            }
        }
    }
}

def sendLogsToSplunk(logs) {
    def splunkUrl = "${env.SPLUNK_HEC_URL}/services/collector/event"
    def token = env.SPLUNK_HEC_TOKEN
    def payload = [
        'event': logs,
        'sourcetype': 'jenkins_log',
        'index': 'main' // Specify the index you want to use in Splunk
    ]
    def jsonPayload = groovy.json.JsonOutput.toJson(payload)

    httpRequest(
        acceptType: 'APPLICATION_JSON',
        contentType: 'APPLICATION_JSON',
        httpMode: 'POST',
        requestBody: jsonPayload,
        url: splunkUrl,
        authentication: 'splunk' // Ensure you have configured this credential in Jenkins
    )
}

