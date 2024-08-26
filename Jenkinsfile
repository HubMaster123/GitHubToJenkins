pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'developer@example.com'
        BUILD_TOOL = 'maven' // or 'gradle' for a Gradle project
        SECURITY_TOOL = 'sonarqube'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the code...'
                    // For Maven:
                    bat 'mvn clean install'
                    // For Gradle:
                    // bat 'gradlew build'
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    // For Maven:
                    bat 'mvn test'
                    // For Gradle:
                    // bat 'gradlew test'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    echo 'Performing code analysis...'
                    // SonarQube example:
                    bat 'sonar-scanner'
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    // Example: OWASP ZAP
                    bat 'zap-cli quick-scan http://your-app-url'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging environment...'
                    // Example: Custom deployment script
                    bat 'deploy-script.bat staging'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    // Example: Custom integration test script
                    bat 'integration-tests.bat staging'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production environment...'
                    // Example: Custom deployment script
                    bat 'deploy-script.bat production'
                }
            }
        }
    }

    post {
        success {
            emailext (
                to: "${EMAIL_RECIPIENT}",
                subject: "Build Successful",
                body: "The build was successful. Please find the build logs attached.",
                attachmentsPattern: "*.log"
            )
        }
        failure {
            emailext (
                to: "${EMAIL_RECIPIENT}",
                subject: "Build Failed",
                body: "The build failed. Please find the build logs attached.",
                attachmentsPattern: "*.log"
            )
        }
    }
}
