pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo "Pulling latest code..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
                sh 'npx playwright install'
            }
        }

        stage('Run Playwright Tests') {
            steps {
                sh 'npx playwright test --reporter=html'
            }
        }

        stage('Publish HTML Report') {
            steps {
                publishHTML target: [
                    reportDir: 'playwright-report',
                    reportFiles: 'index.html',
                    reportName: 'Playwright Test Report',
                    keepAll: true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: true
                ]
            }
        }
    }

    post {
        always {
            echo "Archiving report..."
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
        }
    }
}
