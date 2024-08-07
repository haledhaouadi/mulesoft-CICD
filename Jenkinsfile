pipeline {
    agent any
    tools {
        maven 'maven' // Specify the version of Maven you want to use
        jdk 'Java-8' // Ensure this is set to Java 8 in Jenkins Global Tool Configuration
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
                script {
                    sh "${tool 'maven'}/bin/mvn clean package"
                }
            }
        }
    }
}


