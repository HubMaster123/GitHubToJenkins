pipeline {
    agent any
    
    tools {
        // Define the Maven tool name as configured in Jenkins
        maven3 'Maven 3.9.9' // Ensure this matches the Maven tool name configured in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Display a message indicating the build process
                    echo 'Building the Project...'
                    
                    // Run Maven build command
                    bat '"%MAVEN_HOME%\\bin\\mvn.cmd" clean install'
                }
            }
        }
    }
    
    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
