pipeline {
    agent any 

    stages {
        stage("Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/Chris-deakin/8.2CDevSecOps.git'
            }
        }

        stage("Install Dependencies") {
            steps {
                sh 'npm install'
            }
        }

        stage("Run Tests") {
            steps {
                sh 'npm test || true' // allows pipeline to continue despite test failures
            }
            post {
                always {
                    emailext(
                        
                        to: 'leec8156@gmail.com',
                        subject: "Run Test Stage - ${currentBuild.result ?: 'SUCCESS'}",
                        body: """ 
Test stage completed. 
Job: ${env.JOB_NAME}
Build Num: ${env.BUILD_NUMBER}
Status: ${currentBuild.result ?: 'SUCCESS'}
""",
                        attachLog: true,
                        credentialsId: 'gmail-smtp'
                    )
                }
            }
        }

        stage("Generate Coverage Report") {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage("NPM Audit(Security Scan)") {
            steps {
                sh 'npm audit || true'
            }
            post {
                always {
                    emailext(
                        
                        to: 'leec8156@gmail.com',
                        subject: "Security Scan Stage - ${currentBuild.result ?: 'SUCCESS'}",
                        body: """ 
Security Scan stage completed. 
Job: ${env.JOB_NAME}
Build Num: ${env.BUILD_NUMBER}
Status: ${currentBuild.result ?: 'SUCCESS'}
""",
                        attachLog: true,
                        credentialsId: 'gmail-smtp'
                    )
                }
            }
        }
    }

    post {
        always {
            emailext(
            
                to: 'leec8156@gmail.com',
                subject: "Pipeline Completed - ${currentBuild.result ?: 'SUCCESS'}",
                body: """ 
Pipeline completed. 
Job: ${env.JOB_NAME}
Build Num: ${env.BUILD_NUMBER}
Status: ${currentBuild.result ?: 'SUCCESS'}
""",
                attachLog: true,
                credentialsId: 'gmail-smtp'
            )
        }
    }
}
