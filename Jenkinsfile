pipeline {
    agent any
 
    parameters {
        choice (name : 'ENVIRONMENT',
                choices : ['TEST', 'STAGING','PRODUCTION'],
                description : 'Select Environment for Deployment')
        password  defaultValue: '123ABC', description: 'Enter the API Key', name: 'API_KEY'
        text  defaultValue: 'This is change log', description: 'Enter the change log', name: 'CHANGELOG'
                }
    stages {
        stage ('Building'){
            steps {
                echo "Building"
            }
        }    
        stage ('Deploy to non-production') {
            when {
                //Only deploy if the env not production
                expression { params.ENVIRONMENT != 'PRODUCTION'}
            }
            steps {
                echo 'Building on ${params.ENVIRONMENT}'
                }    
            }
        
        stage ('Deploy to PRODUCTION') {
            when {
                expression {params.ENVIRONMENT == 'PRODUCTION'}
            }
            steps {
                input message : 'Confirm deployment to PRO', ok :'Deploy'
                echo 'Deploying to ${params.ENVIRONMENT}'
                }
            }
        stage ('Report') {
            steps {
                echo "This is a report"
                sh "printf \"${params.CHANGELOG}\" > ${params.ENVIRONMENT}.txt"
                archiveArtifacts allowEmptyArchive : true,
                    artifacts : '*.txt',
                    fingerprint : true,
                    followSymlinks : true,
                    onlyIfSuccessful : true
            }
        }
        
    }
}
