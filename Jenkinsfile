pipeline {
    agent any

    tools {
        nodejs 'node20'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Fetching latest code..."
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

        stage('Publish Report') {
            steps {
                publishHTML([
                    reportDir: 'playwright-report',
                    reportFiles: 'index.html',
                    reportName: 'Playwright Test Report'
                ])
            }
        }
    }

    post {
        always {
            echo "Archiving results..."
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
        }
    }
}
