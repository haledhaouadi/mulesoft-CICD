pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
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
                    }
                }
            }
        }
        stage('SonarQube Check') {
            steps {
                dir('project') { // Assuming the project directory contains the pom.xml file needed for SonarQube analysis
                    script {
                        // Execute SonarQube analysis
                        def sonarOutput = sh(script: "${tool 'maven'}/bin/mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.sources=./src", returnStdout: true).trim()
                        echo "SonarQube Analysis Output:\n${sonarOutput}"
                    }
                }
            }
        }
    }
}

