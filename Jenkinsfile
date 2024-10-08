pipeline 
{
	agent any

	environment 
	{
		EMAIL_RECIPIENT = 'suterliam85@gmail.com'
	}

	stages 
	{
		stage('Build') 
		{
            		steps 
			{
                		script 
				{
	                    		echo 'Building the code...'
	                    		echo 'Using Maven to compile and package the code.'
			    		bat 'echo Simulated Build output > build.log'
                		}
            		}
        	}

	        stage('Unit and Integration Tests') 
		{
	            	steps 
			{
	                	script 
				{
			                echo 'Running unit and integration tests...'
				        echo 'Using JUnit for unit testing and Maven Surefire Plugin for integration tests.'
				        echo "Attempting to send email to: ${EMAIL_RECIPIENT}"
					bat 'echo Simulated test output > test.log'
	                	}
	            	}
	            	post 
			{
			        success 
				{
					archiveArtifacts artifacts: 'test.log'
			                emailext to: "${EMAIL_RECIPIENT}",
				                subject: "Unit and Integration Tests Successful",
				                body: "The Unit and Integration Tests stage completed successfully. Logs are attached.",
						attachmentsPattern: "test.log"
			        }
		                failure 
				{
					archiveArtifacts artifacts: 'test.log'
			                emailext to: "${EMAIL_RECIPIENT}",
				                subject: "Unit and Integration Tests Failed",
				                body: "The Unit and Integration Tests stage failed. Logs are attached.",
						attachmentsPattern: "test.log"
		                }
	            	}
	        }
	
	        stage('Code Analysis') 
		{
	            	steps 
			{
			        script 
				{
				        echo 'Performing code analysis...'
				        echo 'Using SonarQube to analyse the code for quality and industry standards.'
			        }
	            	}
	        }
	
	        stage('Security Scan') 
		{
	        	steps 
			{
	                	script 
				{
		                	echo 'Performing security scan...'
		                	echo 'Using OWASP Dependency Check to scan the code for vulnerabilities.'
		                	echo "Attempting to send email to: ${EMAIL_RECIPIENT}"
					bat 'echo Simulated security scan output > security.log'
	                	}
			}
	            	post 
			{
	                	success 
				{
					archiveArtifacts artifacts: 'security.log'
		                        emailext to: "${EMAIL_RECIPIENT}",
			                        subject: "Security Scan Successful",
			                        body: "The Security Scan stage completed successfully. Logs are attached.",
						attachmentsPattern: "security.log"
	                	}
	                	failure 
				{
					archiveArtifacts artifacts: 'security.log'
		                        emailext to: "${EMAIL_RECIPIENT}",
			                        subject: "Security Scan Failed",
			                        body: "The Security Scan stage failed. Logs are attached.",
			                        attachmentsPattern: "security.log"
	                	}
		        }
	        }
	
	        stage('Deploy to Staging') 
		{
	        	steps 
			{
	                	script 
				{
		                	echo 'Deploying the application to the staging environment...'
		                	echo 'Using custom deployment script for deployment.'
	                	}
	            	}
	        }
	
	        stage('Integration Tests on Staging') 
		{
	            	steps 
			{
	                	script 
				{
		                	echo 'Running integration tests on the staging environment...'
		                	echo 'Using custom integration testing script.'
		                }
	            	}
	        }
	
	        stage('Deploy to Production') 
		{
	            	steps 
			{
	                	script 
				{
		                	echo 'Deploying the application to the production environment...'
		                	echo 'Using custom deployment script for deployment.'
	                	}
	           	}
	        }
	}
}
