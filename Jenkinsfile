pipeline {
    agent any
    tools {
        maven 'sonarmaven'
    }
    environment{
        sonar_token=credentials('s')
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
                bat 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeenv('sonarqube-server'){
                     bat '''
                 mvn sonar:sonar \
                 -Dsonar.projectKey=pipeline3 \
                 -Dsonar.sources=src/main/java \
                 -Dsonar.host.url=http://localhost:9000 \
                 -Dsonar.token=%sonar_token%
                '''
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
