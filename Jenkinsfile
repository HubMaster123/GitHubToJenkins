pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-southeast-2' 
        STAGING_INSTANCE_ID = 'i-0e6aed80d07b7a1d0'
        DEPLOYMENT_SCRIPT_PATH = '/path/to/your/deployment-script.sh'  // Update this path as necessary
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the Project...'
                    sh 'mvn clean install'
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    sh 'mvn test'
                    sh 'mvn verify'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running Checkstyle analysis...'
                sh 'mvn checkstyle:checkstyle'
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
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/dependency-check-report.xml'
                    recordIssues(tools: [dependencyCheck(pattern: '**/dependency-check-report.xml')])
                }
            }                    
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo "Deploying to staging server..."
                    sh """
                        scp -i ~/.ssh/id_rsa ${DEPLOYMENT_SCRIPT_PATH} ec2-user@${STAGING_INSTANCE_ID}:/home/ec2-user/
                    """
                    sh """
                        ssh -i ~/.ssh/id_rsa ec2-user@${STAGING_INSTANCE_ID} 'bash /home/ec2-user/deployment-script.sh'
                    """
                }                
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    // Add the necessary steps for running tests using Selenium, JUnit, or any other testing tool
                    sh 'mvn test -Dtest=IntegrationTest'  // Example for running specific tests
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production server...'
                    // Add your production deployment steps here
                }
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
