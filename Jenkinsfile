pipeline {
    agent any

    tools {
        // Use Maven installed in Jenkins
        maven 'Maven' //'Maven_3.8.6'
        jdk   'jdk'  //'jdk17'
    }

    environment {
        SONARQUBE_ENV = 'SonarQubeServer'  // Name configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/RishiSathe/SonarQuebeDemo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
