pipeline {
    agent any
    
    environment {
        // Define the Maven tool name as configured in Jenkins
        MAVEN_HOME = tool name: 'Maven3', type: 'maven'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Display a message indicating the build process
                    echo 'Building the Project...'
                    
                    // Run Maven build command
                    sh "'${MAVEN_HOME}/bin/mvn' clean install"
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
