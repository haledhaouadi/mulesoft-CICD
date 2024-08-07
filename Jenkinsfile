pipeline {
    stages {
        stage('Build') {        
            steps {
                script {
                    def packageScope
                    if (env.BRANCH_NAME ==~ /^release\//) {
                        packageScope = "${env.BUILD_ID}_RELEASE"
                    } else if (env.BRANCH_NAME == 'Develop' || env.BRANCH_NAME == 'hotfix') {
                        packageScope = "${env.BUILD_ID}_DRAFT"
                    } else {
                        packageScope = "${env.BUILD_ID}_BUILD_Ordinaire"
                    }
                    
                    sh """
                        mvn -s ${env.WORKSPACE}/ci-settings.xml clean package -DpackageScope=${packageScope} -DskipTests | tee ${LOG_FILE_PATH}
                    """
                }
            }
           
        }
      
    }
}
