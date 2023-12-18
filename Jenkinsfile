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
//                     iqOrganization: '14f91ffcb54e4f0d86a158deb383457f',
                    iqStage: 'build',                     
                    iqScanPatterns: [[scanPattern: '**/pom.xml'], [scanPattern: '!module-2/pom.xml'], [scanPattern: '**/*.jar'], [scanPattern: '**/*.properties']],
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
