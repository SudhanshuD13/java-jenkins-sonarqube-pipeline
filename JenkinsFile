pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven'
    }

    environment {
        SONAR_HOST_URL = "http://localhost:9000"
        SONAR_TOKEN = "PASTE_YOUR_NEW_TOKEN_HERE"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SudhanshuD13/java-jenkins-sonarqube-pipeline.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh '''
                mvn sonar:sonar \
                -Dsonar.host.url=$SONAR_HOST_URL \
                -Dsonar.login=$SONAR_TOKEN
                '''
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
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}

