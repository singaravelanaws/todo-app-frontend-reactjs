pipeline {
    agent any
    
    environment {
        DEV_PROJECT = 'prj-contentportal-dev-389901'
	TEST_PROJECT = 'prj-contentportal-test-389901'
        EMAIL_TO = "singaravelan.palani@sifycorp.com"	  
        BRANCH_NAME = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
    }
    stages {
        stage('Build') {
            steps {
                script {
			if (BRANCH_NAME == 'master') {
                      		sh'''
					gcloud config set project ${DEV_PROJECT}
					echo "this is ${BRANCH_NAME}"
				'''
                   	 } else if (BRANCH_NAME == 'test') {
                        	sh'''
					gcloud config set project ${TEST_PROJECT}
					echo "this is ${BRANCH_NAME} branch"
				'''
                    	} else {
                        	error("Unsupported branch ")
                    	}
		      }
            	}
        }
    }
    
    post {
        success {
            script {
            
                def emailTemplate = readFile("email-template.html")
                
                if (BRANCH_NAME == 'master') {
			ENVIRONMENT_NAME = 'Development'
			GIT_COMMIT_ID = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
			GIT_COMMIT_MESSAGES = sh(returnStdout: true, script: 'git log --pretty=format:"%s" $GIT_COMMIT^..$GIT_COMMIT').trim()
			GIT_DATE_MODIFIED = sh(returnStdout: true, script: 'git show -s --format="%ai" $GIT_COMMIT').trim()
			GIT_COMMITTER_NAME = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%cn"').trim()
			GIT_COMMITTER_EMAIL = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ce"').trim()
			GIT_AUTHOR_NAME = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%an"').trim()
			GIT_AUTHOR_EMAIL = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ae"').trim()
			APPLICATION_URL = 'http://crp-dev.sify.digital'
                } else if (BRANCH_NAME == 'test') {
			ENVIRONMENT_NAME = 'Testing'
			GIT_COMMIT_ID = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
			GIT_COMMIT_MESSAGES = sh(returnStdout: true, script: 'git log --pretty=format:"%s" $GIT_COMMIT^..$GIT_COMMIT').trim()
			GIT_DATE_MODIFIED = sh(returnStdout: true, script: 'git show -s --format="%ai" $GIT_COMMIT').trim()
			GIT_COMMITTER_NAME = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%cn"').trim()
			GIT_COMMITTER_EMAIL = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ce"').trim()
			GIT_AUTHOR_NAME = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%an"').trim()
			GIT_AUTHOR_EMAIL = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ae"').trim()
			APPLICATION_URL = 'http://crp-test.sify.digital'
                }
                
                emailTemplate = emailTemplate.replace('${ENVIRONMENT_NAME}', ENVIRONMENT_NAME)
                emailTemplate = emailTemplate.replace('${GIT_COMMIT_ID}', GIT_COMMIT_ID)
		emailTemplate = emailTemplate.replace('${GIT_COMMIT_MESSAGES}', GIT_COMMIT_MESSAGES)
		emailTemplate = emailTemplate.replace('${GIT_DATE_MODIFIED}', GIT_DATE_MODIFIED)
                emailTemplate = emailTemplate.replace('${GIT_COMMITTER_NAME}', GIT_COMMITTER_NAME)
                emailTemplate = emailTemplate.replace('${GIT_COMMITTER_EMAIL}', GIT_COMMITTER_EMAIL)
                emailTemplate = emailTemplate.replace('${GIT_AUTHOR_NAME}', GIT_AUTHOR_NAME)
                emailTemplate = emailTemplate.replace('${GIT_AUTHOR_EMAIL}', GIT_AUTHOR_EMAIL)
                emailTemplate = emailTemplate.replace('${APPLICATION_URL}', APPLICATION_URL)
                emailext(
                    subject: "$JOB_NAME - Build # $BUILD_NUMBER - SUCCESS!!!",
                    to: "${EMAIL_TO}",
                    body: emailTemplate,
                    mimeType: 'text/html',
                    attachLog: true,  
                )
            }
        }
    }
}
