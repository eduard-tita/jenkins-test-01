pipeline {
    agent any
    
    stages {
        stage('Build') { 
            tools {
               jdk 'Java 8'
                maven 'Maven'
            }
            steps {
                //sh 'mvn -B -DskipTests clean package dependency:copy-dependencies' 
                sh 'java -version' 
                sh 'mvn -B -DskipTests clean package'                 
            }
        }
        stage('IQ Policy Evaluation') {
            steps {
                sh 'java -version' 
                nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: selectedApplication('local-iq-app'), 
                    iqStage: 'build', enableDebugLogging: false,
                    iqScanPatterns: [[scanPattern: '**/pom.xml'], [scanPattern: '**/*.jar'], [scanPattern: '**/*.properties']]                
            }
        }
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
