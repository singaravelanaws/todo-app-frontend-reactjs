pipeline {
    agent any
    environment {
        CLOUDSDK_CORE_PROJECT = 'prj-isca-devsecops-test'
        EMAIL_TO = "singaravelan.palani@sifycorp.com"
		    ENVIRONMENT = 'test'
		
    }
    stages {
			stage('Testing') {
				steps {
                 script {
                    sh '''
						      echo "Testing
                    '''
					}
				}
		}
	 }
        post {
                success {
                        emailext mimeType: 'text/html',
                            body: '${FILE, path="/opt/html-template/success/success.html"}',
                            subject: currentBuild.currentResult + " : " + env.JOB_NAME,
                            to: "${EMAIL_TO}"
                         }
                        failure {
                        emailext mimeType: 'text/html',
                            body: '${FILE, path="/opt/html-template/success/success.html"}',
                            subject: currentBuild.currentResult + " : " + env.JOB_NAME,
                            to: "${EMAIL_TO}"
                        }
                }
}
