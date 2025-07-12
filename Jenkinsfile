pipeline {
    agent any
    
    tools {
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-cred', url: 'https://github.com/easycloudcompute/Multi-Tier-BankApp-CI.git']])
            }
        }
    
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
    
        stage('Run unit tests') {
            steps {
                sh "mvn test -DskipTests=true"
            }
        }
    
        stage('File System Scan by Trivy') {
            steps {
                sh "trivy fs --format table -o fs.html ."
            }
        }
    
        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=bankapp -Dsonar.projectKey=bankapp \
                         -Dsonar.java.binaries=target ''' 
                }
            }
        }
    }
}
