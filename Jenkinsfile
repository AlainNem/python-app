pipeline {
    agent any
    
    stages{
        stage('SCM') {
            checkout scm
    }
        stage('SonarQube Analysis') {
            steps {
                script {
                // requires SonarQube Scanner 2.8+
                scannerHome = tool 'SonarScanner'
                }
                withSonarQubeEnv('SonarQube Server') {
                sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=python-app"
                }
            }
    }

        stage('Build Docker Image') {
            steps {
                script{
                    sh 'docker build -t python-http-server .'
            }
        }
    }
        stage('Containerize And Test') {
            steps {
                script{
                    sh 'docker run -d --name python-app python-http-server && sleep 10 && docker stop python-app'
                }
            }
        } 
}
        post {
        always {
            // Always executed
                sh 'docker rm python-app'
        }
        success {
            // on sucessful execution
            sh 'docker logout'   
        }
    }
}