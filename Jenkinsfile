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
                    //Maven Build
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
                // Run Checkstyle with Maven. 
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                always {
                    //Archive Checkstyle reports\
                    archiveArtifacts artifacts: '**/checkstyle-result.xml'
                    recordIssues(tools: [checkStyle(pattern: '**/checkstyle-result.xml')])
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running OWASP Dependency-Check...'
                //Run the Dependency-Check with Maven. 
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
            post {
                always {
                    //Archive Dependency-Check reports
                    archiveArtifacts artifacts: '**/dependency-check-report.xml'
                    recordIssues(tools: [dependencyCheck(pattern: '**/dependency-check-report.xml')])
                }
            }                    
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    // Print the environment variables for debugging
                    echo "Deploying to staging server..."

                    sh """
                        # aws ec2-instance-connect send-ssh-public-key \
                        # --instance-id ${STAGING_INSTANCE_ID} \
                        # --availability-zone us-west-2a \
                        # --instance-os-user ec2-user \
                        # --ssh-public-key file://~/.ssh/id_rsa.pub
                    
                        scp -i ~/.ssh/id_rsa ${DEPLOYMENT_SCRIPT_PATH} ec2-user@${STAGING_INSTANCE_ID}:/home/ec2-user/
                    """
                    
                    // Execute the deployment script on the EC2 instance
                    sh """
                        ssh -i ~/.ssh/id_rsa ec2-user@${STAGING_INSTANCE_ID} 'bash /home/ec2-user/deployment-script.sh'
                    """
                }                
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                echo 'Tool: Selenium or JUnit'  // Or any other tool suitable for testing
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production server...'
                echo 'Production Environment: AWS EC2 instance'  // Or any other production environment
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
