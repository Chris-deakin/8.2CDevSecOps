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
    }
}
