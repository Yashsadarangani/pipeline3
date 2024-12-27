pipeline {
    agent any
    environment {
        PATH = "${PATH};C:\\Windows\\System32"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                bat '''
                pip install -r requirements.txt
                '''
            }
        }
        stage('SonarAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-credential')
            }
            steps {
                bat '''
                sonar-scanner -Dsonar.projectKey=pipeline3 ^
                -Dsonar.sources=. ^
                -Dsonar.host.url=http://localhost:9000 ^
                -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }
    }
    post {
        success {
            echo 'Build Succeeded!'
        }
        failure {
            echo 'Build Failed'
        }
    }
}
