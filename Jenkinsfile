pipeline {
    agent { docker { image 'node:latest' } }
    environment {
        HOME = '.'
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('OWASP_Dependency-Check_Vulnerabilities') {
            steps {
				dependencyCheck additionalArguments: '--format HTML --format XML --suppression suppression.xml', 
				
				// MUST MATCH THE FILE NAME
				odcInstallation: 'OWASP_Dependency-Check_Vulnerabilities'
			}
        }

    }
    post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}