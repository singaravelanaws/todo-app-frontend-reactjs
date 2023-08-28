pipeline {
    agent any
    
    environment {
        CLOUDSDK_CORE_PROJECT = 'prj-contentportal-dev-389901'
        EMAIL_TO = "singaravelan.palani@sifycorp.com"
        ENVIRONMENT = 'Development'
        COMMITID = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
        AUTHOR_FULL = sh(returnStdout: true, script: 'git show -s --format="%an <%ae>" $GIT_COMMIT').trim()
        AUTHOR = AUTHOR_FULL.replaceAll(/ <.*>/, '').trim()
        COMMIT_MESSAGES = sh(returnStdout: true, script: 'git log --pretty=format:"%s" $GIT_COMMIT^..$GIT_COMMIT').trim()
        DATE_MODIFIED = sh(returnStdout: true, script: 'git show -s --format="%ai" $GIT_COMMIT').trim()
        EMAIL_TEMPLATE_FILE = 'success.html'
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'echo ${ENVIRONMENT} ${COMMITID} ${DATE_MODIFIED}'
            }
        }
    }
    
    post {
        always {
            script {
                def emailTemplate = readFile("${EMAIL_TEMPLATE_FILE}")
                emailTemplate = emailTemplate.replace('${ENVIRONMENT}', ENVIRONMENT)
                emailTemplate = emailTemplate.replace('${COMMITID}', COMMITID)
                emailTemplate = emailTemplate.replace('${AUTHOR}', AUTHOR)
                emailTemplate = emailTemplate.replace('${DATE_MODIFIED}', DATE_MODIFIED)
                emailTemplate = emailTemplate.replace('${COMMIT_MESSAGES}', COMMIT_MESSAGES)
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
