pipeline {
    agent any

    tools {
        nodejs 'nodejs-lts'  // Use the exact name from Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/L-Passakorn/simple-express-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'npx sonar-scanner -Dsonar.token=sqp_d13c9cce3f94b624d9fa0f2960de757514784ad3 -Dsonar.projectKey=6510110356_jenkins-sonarqube'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
