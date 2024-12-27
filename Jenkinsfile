pipeline {
    agent any
    tools {
        maven 'sonarmaven' // The Maven installation name
    }
    environment{
        PATH = "${PATH};C:\\Windows\\System32"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar')
            }
            steps {
                withSonarQubeEnv('sonarqube-server') {
                    bat'''
                    mvn sonar:sonar \
                    -Dsonar.projectKey=pipeline4 \
                   -Dsonar.sources=src/main/java \
                   -Dsonar.host.url=http://localhost:9000 \
                   -Dsonar.login=%SONAR_TOKEN%
                   '''
                }
            }
        }
    }
  post {
        success {
            echo 'Build Succeeded!'
        }
        failure {
            echo 'Build Failed'
        }
    }
}
