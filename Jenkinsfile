pipeline {
    agent { label 'Maven' }

    tools {
        maven 'Maven3' // Name of Maven installation in Jenkins
    }

    environment {
        TOMCAT_URL = 'http://54.89.151.231:8080' // Your Tomcat server URL
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Dinesh-SMG/Learning-Experiments.git',
                    credentialsId: 'git-creds' // Use a Jenkins credential ID for Git access
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
                    credentialsId: 'tomcat-user', // Jenkins credential ID for Tomcat (username/password)
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
