pipeline {
    agent any
    tools { maven 'maven' } // Specify the version of Maven you want to use
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


