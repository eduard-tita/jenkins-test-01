pipeline {
    agent any
    
    stages {
        stage('Build') { 
            tools {
                jdk 'Java 17'
                maven 'Maven'
            }
            steps {
                sh 'java -version'
                sh 'mvn -version'
                sh 'mvn -B -DskipTests clean package dependency:copy-dependencies'                             
            }
        }
        stage('IQ Policy Evaluation') {
            steps {
                sh 'java -version' 
                nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: 'iq-app-01', iqStage: 'build',                     
                    iqScanPatterns: [
                        [scanPattern: '**/pom.xml'], 
                        [scanPattern: '**/*.jar'], 
                        [scanPattern: '**/*.properties'], 
                        [scanPattern: 'nexus-java-api-bom.xml']
                    ],
                    enableDebugLogging: false,
                    callflow: [
                      enable: true
                    ]
            }
        }
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
