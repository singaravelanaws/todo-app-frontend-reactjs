
pipeline {
    agent any
    
    environment {
        CLOUDSDK_CORE_PROJECT = 'prj-contentportal-dev-389901'
        EMAIL_TO = "singaravelan.palani@sifycorp.com"
        ENVIRONMENT_NAME = 'Development'
        GIT_COMMIT_ID = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
	GIT_COMMIT_MESSAGES = sh(returnStdout: true, script: 'git log --pretty=format:"%s" $GIT_COMMIT^..$GIT_COMMIT').trim()
	GIT_DATE_MODIFIED = sh(returnStdout: true, script: 'git show -s --format="%ai" $GIT_COMMIT').trim()
        GIT_COMMITTER_NAME = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%cn"').trim()
        GIT_COMMITTER_EMAIL = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ce"').trim()
        GIT_AUTHOR_NAME = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%an"').trim()
        GIT_AUTHOR_EMAIL = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ae"').trim()
	APPLICATION_URL = 'http://crp-dev.sify.digital
    }
    
    stages {
        stage('Build') {
            steps {
                // Your build steps here
            }
        }
    }
    
    post {
        always {
            script {
                def emailTemplate = readFile("success.html")
                emailTemplate = emailTemplate.replace('${ENVIRONMENT_NAME}', ENVIRONMENT_NAME)
                emailTemplate = emailTemplate.replace('${GIT_COMMIT_ID}', GIT_COMMIT_ID)
		emailTemplate = emailTemplate.replace('${GIT_COMMIT_MESSAGES}', GIT_COMMIT_MESSAGES)
		emailTemplate = emailTemplate.replace('${GIT_DATE_MODIFIED}', GIT_DATE_MODIFIED)
                emailTemplate = emailTemplate.replace('${GIT_COMMITTER_NAME}', GIT_COMMITTER_NAME)
                emailTemplate = emailTemplate.replace('${GIT_COMMITTER_EMAIL}', GIT_COMMITTER_EMAIL)
                emailTemplate = emailTemplate.replace('${GIT_AUTHOR_NAME}', GIT_AUTHOR_NAME)
                emailTemplate = emailTemplate.replace('${GIT_AUTHOR_EMAIL}', GIT_AUTHOR_EMAIL)
                emailTemplate = emailTemplate.replace('${NODE_NAME}', NODE_NAME)
                emailext(
                    subject: "Build Notification - ${ENVIRONMENT}",
                    to: "${EMAIL_TO}",
                    body: emailTemplate,
                    mimeType: 'text/html',
                )
            }
        }
    }
}
