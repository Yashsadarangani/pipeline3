pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://localhost:9000' // Replace with your SonarQube URL
        SONAR_AUTH_TOKEN = credentials('sonar-credential') // Replace with your Jenkins credential ID for SonarQube
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the GitHub repository
                git branch: 'main', url: 'https://github.com/Yashsadarangani/pipeline3.git', credentialsId: 'github-credential'
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
                bat '''
                sonar-scanner ^
                -Dsonar.projectKey=pipeline3 ^
                -Dsonar.sources=. ^
                -Dsonar.host.url=http://localhost:9000 ^
                -Dsonar.token=sqp_52c7e64662736865234ae805bc392bdab549b68e
                '''
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
