pipeline {
    agent any
    tools {
        maven 'sonarmaven' // Ensure this matches the Maven tool name in Jenkins
    }
    environment {
        PATH = "${PATH};C:\\Windows\\System32" // Ensure this is necessary
        SONAR_TOKEN = credentials('jenkins-token') // Replace with your actual credential ID
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
