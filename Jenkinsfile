pipeline {
    agent any
    
    environment {
        CLOUDSDK_CORE_PROJECT = 'prj-contentportal-dev-389901'
        EMAIL_TO = "singaravelan.palani@sifycorp.com"
        ENVIRONMENT = 'Development'
        COMMITID = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
        AUTHOR = sh(returnStdout: true, script: 'git show -s --format="%an <%ae>" $GIT_COMMIT').trim()
        DATE_MODIFIED = sh(returnStdout: true, script: 'git show -s --format="%ai" $GIT_COMMIT').trim()
        EMAIL_TEMPLATE_FILE = 'success.html'
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
                def emailTemplate = readFile("${EMAIL_TEMPLATE_FILE}")
                
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
