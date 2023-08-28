pipeline {
    agent any

    environment {
        DEV_PROJECT = 'prj-contentportal-dev-389901'
        TEST_PROJECT = 'prj-contentportal-test-389901'
        EMAIL_TO = 'singaravelan.palani@sifycorp.com'
        BRANCH_NAME = env.GIT_BRANCH ?: 'unknown'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    def project
                    def environmentName

                    switch (BRANCH_NAME) {
                        case 'refs/heads/master':
                            project = DEV_PROJECT
                            environmentName = 'Development'
                            break
                        case 'refs/heads/test':
                            project = TEST_PROJECT
                            environmentName = 'Testing'
                            break
                        default:
                            error("Unsupported branch: ${BRANCH_NAME}")
                    }

                    sh "gcloud config set project ${project}"
                    echo "This is ${BRANCH_NAME}"
                }
            }
        }
    }

    post {
        success {
            script {
                def emailTemplate = readFile("email-template.html")
                def gitCommitId = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                def gitCommitMessages = sh(returnStdout: true, script: "git log --pretty=format:\"%s\" ${gitCommitId}").trim()
                def gitDateModified = sh(returnStdout: true, script: "git show -s --format=\"%ai\" ${gitCommitId}").trim()
                def gitCommitterName = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%cn"').trim()
                def gitCommitterEmail = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ce"').trim()
                def gitAuthorName = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%an"').trim()
                def gitAuthorEmail = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ae"').trim()
                def applicationUrl

                switch (BRANCH_NAME) {
                    case 'refs/heads/master':
                        environmentName = 'Development'
                        applicationUrl = 'http://crp-dev.sify.digital'
                        break
                    case 'refs/heads/test':
                        environmentName = 'Testing'
                        applicationUrl = 'http://crp-test.sify.digital'
                        break
                }

                emailTemplate = emailTemplate.replace('${ENVIRONMENT_NAME}', environmentName)
                emailTemplate = emailTemplate.replace('${GIT_COMMIT_ID}', gitCommitId)
                emailTemplate = emailTemplate.replace('${GIT_COMMIT_MESSAGES}', gitCommitMessages)
                emailTemplate = emailTemplate.replace('${GIT_DATE_MODIFIED}', gitDateModified)
                emailTemplate = emailTemplate.replace('${GIT_COMMITTER_NAME}', gitCommitterName)
                emailTemplate = emailTemplate.replace('${GIT_COMMITTER_EMAIL}', gitCommitterEmail)
                emailTemplate = emailTemplate.replace('${GIT_AUTHOR_NAME}', gitAuthorName)
                emailTemplate = emailTemplate.replace('${GIT_AUTHOR_EMAIL}', gitAuthorEmail)
                emailTemplate = emailTemplate.replace('${APPLICATION_URL}', applicationUrl)

                emailext(
                    subject: "${JOB_NAME} - Build # ${BUILD_NUMBER} - SUCCESS!!!",
                    to: "${EMAIL_TO}",
                    body: emailTemplate,
                    mimeType: 'text/html',
                    attachLog: true
                )
            }
        }
    }
}
