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
                    // Use the SonarQube credentials configured in Jenkins (withSonarQubeEnv)
                    sh 'npx sonar-scanner -Dsonar.projectKey=6510110356_jenkins-sonarqube'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Increase timeout to allow SonarQube background processing to complete
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
