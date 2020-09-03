pipeline {
	agent any
	stages{
	        stage('Lint HTML') {
              		steps {
                  		sh 'tidy -q -e *.html'
              		}
                }

		stage('Upload to AWS') {
			steps {
				withAWS(region:'ap-south-1', credentials: 'aws-static') {
					sh 'echo "Uploading content with AWS creds"'
					s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'jenkins9582')
				}
			}
		}
		stage('Site Up check'){
			steps {
               		script {
                    		final String url = "http://jenkins9582.s3-website.ap-south-1.amazonaws.com/"

                    		final String response = sh(script: "curl -I $url|grep 'HTTP/1.1 200'", returnStdout: true).trim()

                    		echo response
                	}

			
		}
	}
}
