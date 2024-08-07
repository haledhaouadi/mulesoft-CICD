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
                        sh "${tool 'maven'}/bin/mvn clean package -e -X"
                    }
                }
            }
        }
    }
}



