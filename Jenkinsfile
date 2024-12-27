pipeline {
    agent any
    tools {
        maven 'sonarmaven' // Ensure this matches the Maven tool name in Jenkins
    }
    environment {
        PATH = "${PATH};C:\\Windows\\System32" // Ensure this is necessary
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the repository...'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                bat 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-credentials') // Replace with your actual credential ID
            }
            steps {
                echo 'Starting SonarQube analysis...'
                withSonarQubeEnv('sonarqube-server') { // Ensure 'sonarqube-server' is configured in Jenkins
                    bat """
                    mvn sonar:sonar ^
                        -Dsonar.projectKey=pipeline3 ^
                        -Dsonar.sources=src/main/java ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.login=%SONAR_TOKEN%
                    """
                }
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
