pipeline {
    agent { label 'New Node' }  // Use your Jenkins agent named "New Node"
    
    tools {
        jdk 'JDK17'        // Make sure JDK17 is configured under Jenkins -> Global Tool Configuration
        maven 'Maven3'     // Make sure Maven is configured too
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'git@github.com:Dinesh-SMG/Learning-Experiments.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Archive JAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build successful! The JAR file has been archived and is available for download.'
        }
        failure {
            echo '❌ Build failed! Please check the console output.'
        }
    }
}
