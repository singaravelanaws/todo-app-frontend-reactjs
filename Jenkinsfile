pipeline {
    agent any
    
    environment {
        CLOUDSDK_CORE_PROJECT = 'prj-contentportal-dev-389901'
        EMAIL_TO = "singaravelan.palani@sifycorp.com"
        ENVIRONMENT = 'Development'
        COMMITID = sh(returnStdout: true, script: 'git show -s --format="%an <%ae>" $GIT_COMMIT').trim()
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
                emailTemplate = emailTemplate.replaceAll('\\${DATE_MODIFIED}', DATE_MODIFIED)
                emailTemplate = emailTemplate.replaceAll('\\${ENVIRONMENT}', ENVIRONMENT)
                emailTemplate = emailTemplate.replaceAll('\\${COMMITID}', COMMITID)
                emailTemplate = emailTemplate.replaceAll('\\${AUTHOR}', AUTHOR)
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
