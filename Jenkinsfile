pipeline {
    agent any

    environment {
        DEV_PROJECT = 'prj-contentportal-dev-389901'
        TEST_PROJECT = 'prj-contentportal-test-389901'
        EMAIL_TO = 'singaravelan.palani@sifycorp.com'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    def project
                    def environmentName
                    def gitBranch = env.GIT_BRANCH ?: 'unknown'

                    switch (gitBranch) {
                        case 'origin/master':
                            project = DEV_PROJECT
                            environmentName = 'Development'
                            break
                        case 'origin/test':
                            project = TEST_PROJECT
                            environmentName = 'Testing'
                            break
                        default:
                            error("Unsupported branch: ${gitBranch}")
                    }

                    sh "gcloud config set project ${project}"
                    echo "This is ${gitBranch}"
                }
            }
        }
    }

    post {
        success {
            script {
                def emailTemplate = readFile("email-template.html")
                def gitCommitId = h(returnStdout: true, script: 'git rev-parse HEAD').trim()
                def gitCommitMessages = sh(returnStdout: true, script: 'git log --pretty=format:"%s" $GIT_COMMIT^..$GIT_COMMIT').trim()
                def gitDateModified = sh(returnStdout: true, script: "git show -s --format=\"%ai\" ${gitCommitId}").trim()
                def gitCommitterName = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%cn"').trim()
                def gitCommitterEmail = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ce"').trim()
                def gitAuthorName = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%an"').trim()
                def gitAuthorEmail = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%ae"').trim()
                def applicationUrl
                def environmentName
                def emailTemplate = readFile("email-template.html")


                def gitBranch = env.GIT_BRANCH ?: 'unknown'

                switch (gitBranch) {
                    case 'origin/master':
                        environmentName = 'Development'
                        applicationUrl = 'http://crp-dev.sify.digital'
                        break
                    case 'origin/test':
                        environmentName = 'Testing'
                        applicationUrl = 'http://crp-test.sify.digital'
                        break
                }

                if (!environmentName) {
                    error("Unsupported branch: ${gitBranch}")
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
