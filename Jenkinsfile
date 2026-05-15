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
            steps{
                sh 'npm test || true' //allows pipeline to continue despite test failures
            }
        }
        post{
            always{
                emailext(
                    subject: "Test Stage - ${currentBuild.currentResult}",
                    body: """ 
                    Test stage completed. 
                    Job: ${env.JOB_NAME}
                    Build Num: ${env.BUILD_NUMBER}
                    Status: ${currentBuild.currentResult}
                    """,
                    to: 'leec8156@gmail.com',
                    attachLog: true

                )
            }
        }

        stage("Generate Coverage Report"){
            steps{
                sh 'npm run coverage || true'
            }
        }

        stage("NPM Audit(Security Scan)") {
            steps {
                sh 'npm audit || true'
            }
        }
        
        post{
            always{
                emailext(
                    subject: "Test Stage - ${currentBuild.currentResult}",
                    body: """ 
                    Test stage completed. 
                    Job: ${env.JOB_NAME}
                    Build Num: ${env.BUILD_NUMBER}
                    Status: ${currentBuild.currentResult}
                    """,
                    to: 'leec8156@gmail.com',
                    attachLog: true

                )
            }
    }
