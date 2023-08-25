pipeline {
    agent any
    environment {
            CLOUDSDK_CORE_PROJECT = 'prj-contentportal-dev-389901'
            EMAIL_TO = "singaravelan.palani@sifycorp.com"	
	    RELEASE_NOTES = sh (script: """git log --format="medium" -1 ${GIT_COMMIT}""", returnStdout:true)
    }
    stages {
       // Node JS Code Build 
          stage('Echo'){
            steps{
                        sh '''
                            echo $RELEASE_NOTES
                        '''
                    }
       }
    }
        post {
             success {
                    emailext mimeType: 'text/html',
                    body: '${FILE, path="success.html"}',
                    subject: currentBuild.currentResult + " : " + env.JOB_NAME,
                    to: "${EMAIL_TO}"
            }
             failure {
                    emailext mimeType: 'text/html',
                    body: '${FILE, path="success.html"}',
                    subject: currentBuild.currentResult + " : " + env.JOB_NAME,
                    to: "${EMAIL_TO}"
            }
        }
}
