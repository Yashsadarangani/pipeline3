pipeline {
    agent any
    tools {
        maven 'Maven 3'
        jdk 'JDK 11'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/username/repository.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                junit 'pom.xml'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                bat '''
                 sonar-scanner -Dsonar.projectKey=pipeline3 ^
                -Dsonar.sources=. ^
                -Dsonar.host.url=http://localhost:9000 ^
                -Dsonar.token=sqa_e0d66921a5e37d4859d748d025d4fe0c23afcbc7
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
