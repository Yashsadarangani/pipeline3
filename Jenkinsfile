pipeline {
    agent any
    environment {
        SONAR_HOST_URL = 'http://localhost:9000' // Replace with your SonarQube URL
        SONAR_AUTH_TOKEN = credentials('sonar-credential') // Replace with your Jenkins credential ID for SonarQube
    }

    stages {
        stage('Checkout Code') {
            steps {
               checkout scm
            }
        }

        stage('Build') {
            steps {
                // Build the Maven project using the pom.xml
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            when {
                expression {
                    return fileExists('pom.xml') // Perform analysis only if pom.xml exists
                }
            }
        steps {
            withSonarQubeEnv('sonarqube-server') { // Replace 'sonarqube-server' with your SonarQube server name
                sh '''
                mvn sonar:sonar \
                -Dsonar.projectKey=pipeline3 \
                -Dsonar.projectName='pipeline3' \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.login=sqa_e0d66921a5e37d4859d748d025d4fe0c23afcbc7
            '''
                }    
            }
        }

        stage('Post-Build') {
            steps {
                echo 'Build and analysis completed successfully!'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
