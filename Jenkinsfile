pipeline {
    agent any
    tools { maven 'maven-3.6.3' } // Specify the version of Maven you want to use
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
                    sh "${tool 'maven-3.6.3'}/bin/mvn clean package"
                }
            }
        }
    }
}


