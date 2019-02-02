pipeline {
    agent any
    stages {
        stage ('Build Servlet Project') {
            steps {
                sh  'mvn clean package'
            }

            post{
                success{
                    echo 'Now Archiving ....'

                    archiveArtifacts artifacts : '**/*.war'
                }
            }
        }
	
	stage ('Deploy build in stage area') {
		steps {
			build job : 'Build-Stage-Area'
		}
	}
	
	stage ('Deploy to production' {
		steps {
			timeout (time:5, unit: 'day'){
				input message: 'Approve production deployment?'
			}
			build job: 'Deploy-Prod-pipeline'
		}
		post {
			success {
				echo 'Deployment to prod is successful'
			}
			failure {
				echo 'Deployment to prod is failed'
			}
		}
	}
   } 
}
