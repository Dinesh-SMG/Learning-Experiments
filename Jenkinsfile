pipeline {
    agent { label 'Maven' }

    tools {
        maven 'Maven3' // Replace with your exact Maven installation name in Jenkins
    }

    environment {
        TOMCAT_URL = 'http://54.89.151.231:8080' // Your Tomcat URL
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Dinesh-SMG/Learning-Experiments.git',
                    credentialsId: 'tomcat-user'
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
                    credentialsId: 'tomcat-user',
                    path: '',
                    url: "${env.TOMCAT_URL}"
                )], contextPath: 'CurrentTimeApp', war: '**/target/*.war'
            }
        }
    }

    post {
        success {
            echo "✅ Build & Deployment Successful! Visit ${env.TOMCAT_URL}/CurrentTimeApp/time"
        }
        failure {
            echo "❌ Build or Deployment Failed!"
        }
    }
}
