pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-southeast-2' 
        STAGING_INSTANCE_ID = 'i-0e6aed80d07b7a1d0'
        DEPLOYMENT_SCRIPT_PATH = ''
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the Project...'
                    bat 'mvn clean install'
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    bat 'mvn test'
                    bat 'mvn verify'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running Checkstyle analysis...'
                bat 'mvn checkstyle:checkstyle'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/checkstyle-result.xml'
                    recordIssues(tools: [checkStyle(pattern: '**/checkstyle-result.xml')])
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running OWASP Dependency-Check...'
                bat 'mvn org.owasp:dependency-check-maven:check'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/dependency-check-report.xml'
                    recordIssues(tools: [dependencyCheck(pattern: '**/dependency-check-report.xml')])
                }
            }                    
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                echo 'Tool: Selenium or JUnit'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production server...'
                echo 'Production Environment: AWS EC2 instance'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
