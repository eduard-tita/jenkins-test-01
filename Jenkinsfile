pipeline {
    agent any

    tools {
        maven "Maven"
    }

    stages {
        stage('IQ Policy Evaluation') {
            steps {
                nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: selectedApplication('local-iq-app'), 
                    iqStage: 'build', iqScanPatterns: [[scanPattern: '**/pom.xml']]                
            }
        }
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
