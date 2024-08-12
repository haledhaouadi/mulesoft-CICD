pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
        sonarQube 'SonarQube' // Name of the SonarQube scanner tool
    }
    environment {
        SPLUNK_HEC_URL = 'http://localhost:8088'
        SPLUNK_HEC_TOKEN = 'ab2d52f7-3947-4527-a411-33f17c6414'
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
                dir('project') {
                    script {
                        def buildOutput = sh(script: "${tool 'maven'}/bin/mvn clean package -e -X", returnStdout: true).trim()
                        echo buildOutput
                        def jsonPayload = """{
                            "event": "Build logs: ${buildOutput.replaceAll('"', '\\"')}" 
                        }"""
                        sh(script: """
                        curl -k "${env.SPLUNK_HEC_URL}/services/collector" \
                        -H "Authorization: Splunk ${env.SPLUNK_HEC_TOKEN}" \
                        -d '${jsonPayload}'
                        """)
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis
                    sh "${tool 'sonarQube'}/bin/sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=src -Dsonar.host.url=${env.SONARQUBE_URL} -Dsonar.login=${env.SONARQUBE_TOKEN}"
                }
            }
        }
    }
}


