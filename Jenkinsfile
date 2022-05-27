pipeline {
    agent any

    tools {
        maven "Maven"
    }

    stages {
        stage('Build') { 
            steps {
                //sh 'mvn -B -DskipTests clean package dependency:copy-dependencies' 
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('IQ Policy Evaluation') {
            steps {
                //nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: selectedApplication('local-iq-app'), 
                //    iqStage: 'build', iqScanPatterns: [[scanPattern: '**/pom.xml'], [scanPattern: '**/*.jar']]
                //, enableDebugLogging: true                
                
                nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: selectedApplication('local-iq-app'), 
                    iqStage: 'build', iqScanPatterns: [[scanPattern: 'scan_nothing']]                
            }
        }
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
