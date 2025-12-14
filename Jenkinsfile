pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven'
    }

    environment {
        SONAR_PROJECT_KEY = 'com.devops:sonar-demo'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SudhanshuD13/java-jenkins-sonarqube-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                      mvn sonar:sonar \
                      -Dsonar.projectKey=${sqa_9e892733c4ca24a1ae56bf05a3a9f1107ffd4238}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline SUCCESS – Quality Gate Passed'
        }
        failure {
            echo '❌ Pipeline FAILED – Check logs'
        }
    }
}
