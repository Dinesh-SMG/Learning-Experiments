pipeline {
    agent any

    tools {
        maven 'Maven'  // Name must match your Jenkins Maven installation
    }

    environment {
        TOMCAT_URL = 'http://54.89.151.231:8080'  // Your Tomcat EC2 instance public IP
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Dinesh-SMG/Learning-Experiments.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat8(
                    credentialsId: 'tomcat-user', // Your Jenkins credentials ID for Tomcat user
                    path: '',
                    url: "${env.TOMCAT_URL}"
                )], contextPath: 'myapp', war: '**/target/*.war'
            }
        }
    }

    post {
        success {
            echo "✅ Build & Deployment Successful! Visit ${env.TOMCAT_URL}/myapp"
        }
        failure {
            echo "❌ Build or Deployment Failed!"
        }
    }
}
