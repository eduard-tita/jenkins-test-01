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
                    iqOrganization: '6cac5ffe36da4de78eb657b8f9a348f7',
                    iqStage: 'build',                     
                    iqScanPatterns: [[scanPattern: '**/pom.xml'], [scanPattern: '**/*.jar'], [scanPattern: '**/*.properties']],
                    enableDebugLogging: false
            }
        }
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
