pipeline {
 agent any
    environment {
        PROJECT_VERSION = get_version(GIT_BRANCH)
        NPM_VERSION = "${PROJECT_VERSION.split('\\.')[0]}.${PROJECT_VERSION.split('\\.')[1]}.${BUILD_NUMBER}"
		 }
    stages {
            stage('NPM Build') {
                steps {
                            bat 'npm version ' + NPM_VERSION
                            bat 'npm install'
                        
                    }
                
            }
            
    }
        post {
            success {
                script{							
                    def message = "New Version " + PROJECT_VERSION + " - " + BRANCH_NAME

                    if(GIT_BRANCH == "master" || GIT_BRANCH == "develop"){
                        dir('lib'){
                            bat 'npm publish'
                        }
                    }
                }

                archiveArtifacts artifacts: 'lib/dist/**/*', fingerprint: true   
            }
        }

        options {
             // make sure we only keep 50 builds at a time, so we don't fill up our storage!
                buildDiscarder(logRotator(numToKeepStr:'50'))
        }
    
}
