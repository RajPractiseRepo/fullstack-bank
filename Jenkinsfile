pipeline {
    agent any
    
    environment{
        
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RajPractiseRepo/fullstack-bank.git'
            }
        }
        
        stage('Owasp FS Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('Trivy FS Scan') {
            steps {
                
            sh "trivy fs ."
            
            }
        }
        
        stage('SONARQUBE ANALYSIS') {
            steps {
                
            withSonarQubeEnv('sonar') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank "
                     }
            
            }
        }
    }
}
