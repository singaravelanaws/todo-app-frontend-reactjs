pipeline {
    agent any
    environment {
            CLOUDSDK_CORE_PROJECT = 'prj-contentportal-dev-389901'
            EMAIL_TO = "singaravelan.palani@sifycorp.com"	
	   def COMMITID = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
           def AUTHOR = sh(returnStdout: true, script: 'git show -s --format="%an <%ae>" $GIT_COMMIT').trim()
           def DATE_MODIFIED = sh(returnStdout: true, script: 'git show -s --format="%ai" $GIT_COMMIT').trim()
	    //RELEASE_NOTES = sh (script: """git log --format="medium" -1 ${GIT_COMMIT}""", returnStdout:true)
    }
    stages {
       // Node JS Code Build 
          stage('Echo'){
            steps{
                        sh '''
                            echo $commitId
			    echo $author
       			    echo $date
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
