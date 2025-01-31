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
                    def result
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        result = nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: 'iq-app-01', iqStage: 'build',                     
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
                    echo "Scan ID: ${result.scanId} |"
                }
                echo "Env Scan ID: ${env.SONATYPE_IQ_SCAN_ID} |"
            }
        }
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
