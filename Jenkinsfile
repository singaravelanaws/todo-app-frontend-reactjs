pipeline {
    agent any
    environment {
            CLOUDSDK_CORE_PROJECT = 'prj-contentportal-dev-389901'
            EMAIL_TO = "singaravelan.palani@sifycorp.com"	
	    commitMsg = "commit.substring( commit.indexOf(' ') ).trim()"
       	    commitid = 'sh(returnStdout: true, script: 'git log -1 --oneline').trim()'
    }
    stages {
       // Node JS Code Build 
          stage('Echo'){
            steps{
                        sh '''
                            echo $commitMsg
			    echo $commitid
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
