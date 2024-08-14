pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
    }
    environment {
        MAVEN_OPTS = '--add-opens java.base/java.lang=ALL-UNNAMED --add-opens java.base/java.io=ALL-UNNAMED'
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
                    }
                }
            }
        }
        stage('SonarQube Check') {
            steps {
                dir('project') {
                    script {
                        def sonarOutput = sh(script: "${tool 'maven'}/bin/mvn sonar:sonar -Dsonar.host.url=http://192.168.1.118:9000 -Dsonar.sources=./src", returnStdout: true).trim()
                        echo "SonarQube Analysis Output:\n${sonarOutput}"
                    }
                }
            }
        }
        stage('Deploy To CloudHub') {
            steps {
                dir('project'){
                    script{
                         sh 'mvn -X deploy -DmuleDeploy -DskipTests'
                    }
               
                 }
            }
    }
}

