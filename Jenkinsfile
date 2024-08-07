pipeline {
    agent any
    tools { 
        maven 'maven'
        jdk 'Java-8' 
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


