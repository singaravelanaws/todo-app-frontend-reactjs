pipeline {
    agent any
    environment {
            CLOUDSDK_CORE_PROJECT = 'prj-contentportal-dev-389901'
            EMAIL_TO = "singaravelan.palani@sifycorp.com"
	    ENVIRONMENT = 'Development'
	    def COMMITID = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
            def AUTHOR = sh(returnStdout: true, script: 'git show -s --format="%an <%ae>" $GIT_COMMIT').trim()
            def DATE_MODIFIED = sh(returnStdout: true, script: 'git show -s --format="%ai" $GIT_COMMIT').trim()
    }
    stages {
       // Node JS Code Build 
          stage('Echo'){
            steps{
                        sh '''
                            echo $COMMITID
			    echo $AUTHOR
       			    echo $DATE_MODIFIED
	                    echo ${ENVIRONMENT}
                        '''
                    }
       }
    }
    post {
        always {
            script {
                def emailTemplate = "<!DOCTYPE html> <html> <head> <meta http-equiv="Content-Type" content="text/html; charset=utf-8"> <title>Build from Jenkins</title> <style> /* Base Styles */ body { font-family: Arial, sans-serif; background-color: #e4e4e4; margin: 0; padding: 0; } a { color: #4a72af; text-decoration: none; } table.width-100 td { text-align: center; } .width-100 { width: 100%; } /* News Section */ .news { text-align: center; padding-top: 15px; } /* Content Section */ .content { width: 720px; background-color: white; margin: 0 auto; border-radius: 6px; margin-top: 10px; box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); padding: 20px; } /* Status Section */ .status { background-color: #4caf50; font-size: 28px; font-weight: bold; color: white; width: 100%; height: 52px; margin-bottom: 18px; text-align: center; vertical-align: middle; border-collapse: collapse; background-repeat: no-repeat; } .status .info { color: white; font-size: 32px; line-height: 36px; padding: 8px 0; text-shadow: 0 -1px 0 rgba(0, 0, 0, 0.3); } /* Main Content */ .main img { width: 40px; margin: 0 auto; height: auto; } .main table { font-size: 14px; margin-top: 20px; } .main table th { text-align: right; } /* Bottom Message */ .bottom-message { width: 720px; cellpadding: 5px; cellspacing: 0px; margin-top: 20px; } .bottom-message .message { font-size: 13px; color: #aaa; line-height: 18px; text-align: center; } .bottom-message .designed { font-size: 13px; color: #aaa; line-height: 18px; font-style: italic; text-align: right; } .cartoon { width: 36px; display: block; } .centerImage { text-align: center; display: block; } </style> </head> <body> <div class="news"> <p>Do you like the new format of build information? <a href="mailto:singaravelan.palani@sifycorp.com">Give me feedback</a></p> <br> </div> <div class="content"> <div class="status"> <p class="info">${PROJECT_NAME}</p> </div> <div class="main"> <table> <tbody> <tr> <th>JOB_NAME:</th> <td>${PROJECT_NAME}</td> </tr> <tr> <th>BRANCH:</th> <td>${GIT_BRANCH}</td> </tr> <tr> <th>COMMIT_ID:</th> <td>${COMMITID }</td> </tr> <tr> <th>AUTHOR:</th> <td>${AUTHOR}</td> </tr> <tr> <th>DATE_MODIFIED:</th> <td>${DATE_MODIFIED}</td> </tr> <tr> <th>BUILD_URL:</th> <td><a href="${BUILD_URL}">${BUILD_URL}</a></td> </tr> <tr> <th>BUILD_NO:</th> <td>${BUILD_NUMBER}</td> </tr> <tr> <th>JENKINS_URL:</th> <td><a href="${JENKINS_URL}">${JENKINS_URL}</a></td> </tr> </tbody> </table> <div class=""> <img src="https://icons.veryicon.com/png/o/miscellaneous/8atour/success-35.png" class="centerImage" alt="CH Logo"> </div> <table class="bottom-message width-100"> <tbody> <tr> <td> <div> <img class="cartoon" src="https://brandeps.com/logo-download/T/Times-of-India-logo-vector-01.svg"> </div> </td> <td class="message"> <div class="message-footer-content"> <b>${PROJECT_NAME}<br> <a href="https://www.sifytechnologies.com/">Sify Technologies.</a> </b> </div> </td> <td class="message"> <div class="message-footer"> <b>Times of India (TOI)<br> <a href="https://support.sifycorp.com/">Support Team</a> </b> </div> </td> </tr> </tbody> </table> </div> </div> </body> </html>"
                emailext(
                    subject: "TOI Non-Prod ${PROJECT_NAME} Notification - ${ENVIRONMENT}",
                    to: "${EMAIL_TO}",
                    body: emailTemplate,
                    mimeType: 'text/html',
                )
            }
        }
    }
}
