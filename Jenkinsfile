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
                script {                    
                    // catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    try {
                        def result = nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: 'iq-app-01', iqStage: 'build',                     
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
                        echo "result: ${result}"
                    } catch (error) {
                        def result = error.policyEvaluation   
                        echo "result on exception: ${result}"
                    }
                }
            }
        }
        stage('After IQ Policy Evaluation') {
            steps {                
                echo "Env Scan ID: ${env.SONATYPE_IQ_SCAN_ID}"
            }
        }
    }
    
    post {
        always {
            echo "Env Scan ID: ${env.SONATYPE_IQ_SCAN_ID}"
            deleteDir()
        }
    }
}
