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
                nexusPolicyEvaluation failBuildOnNetworkError: false, 
                    iqApplication: 'iq-app-01', 
                    iqStage: 'build',                     
//                    iqScanPatterns: [[scanPattern: '**'], [scanPattern: '!module-2/**/*.*']],                    
//                    iqScanPatterns: [[scanPattern: '**/pom.xml'], [scanPattern: '!module-2/pom.xml']],                                        
                    iqScanPatterns: [[scanPattern: '**/pom.xml'], [scanPattern: '!module-2/pom.xml'], [scanPattern: '**/*.jar'], [scanPattern: '**/*.properties']]
//                    iqScanPatterns: [[scanPattern: 'scan-nothing']],                    
//                    enableDebugLogging: true
            }
        }
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
