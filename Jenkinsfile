pipeline {
    agent any

    tools{
        jdk 'jdk17'
        nodejs 'node16'
    
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

       stage('Install Dependencies') {
            steps {
                
            sh "npm install"
            
            }
        }
        
        stage('Backend') {
            steps {
                dir('/var/lib/jenkins/workspace/fullstack-bank/app/backend') {
                    sh "npm install"
                }
            }
        }
        
        stage('frontend') {
            steps {
                dir('/var/lib/jenkins/workspace/fullstack-bank/app/frontend') {
                    sh "npm install"
                }
            }
        }
        
      
    }
}
