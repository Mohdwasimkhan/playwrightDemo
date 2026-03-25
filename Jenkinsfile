pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    environment {
        CI = 'true'
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Install Dependencies') {
            steps {
                bat 'npm ci'
                bat 'npx playwright install'
            }
        }

        stage('Run Smoke Tests') {
            steps {
                bat 'npm run smoke'
            }
        }

        stage('Publish JUnit Report') {
            steps {
                junit 'results/junit.xml'
            }
        }
    }

    post {
        always {
            allure([
                commandline: 'Allure',
                includeProperties: false,
                results: [[path: 'allure-results']]
            ])
        }
    }
}