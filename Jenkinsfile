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
        stage ('Deploy Build in Staging Area'){
            steps{
                build job : 'Build-Stage-Area'
            }
        }
        stage ('Deploy to Production'){
            steps{
                timeout (time: 5, unit:'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }
                build job : 'Deploy-Prod-pipeline'
            }
            post{
                success{
                    echo 'Deployment on PRODUCTION is Successful'
                }
                failure{
                    echo 'Deployement Failure on PRODUCTION'
                }
            }
        }
    }
}
