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

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
                sh 'npx playwright install --with-deps'
            }
        }

        stage('Run Smoke Tests') {
            steps {
                sh 'npm run smoke'
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
                includeProperties: false,
                jdk: '',
                results: [[path: 'allure-results']]
            ])
        }
    }
}