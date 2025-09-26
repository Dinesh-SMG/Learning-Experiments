pipeline {
    agent { label 'New Node' }  // Run on your specific agent

    tools {
        jdk 'Java21'  // Use your installed JDK
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Pull source code from GitHub
                git branch: 'main', url: 'git@github.com:Dinesh-SMG/Learning-Experiments.git'
            }
        }

        stage('Compile Java') {
            steps {
                // Compile the Java file
                sh 'javac largestno.java'
            }
        }

        stage('Create Executable JAR') {
            steps {
                // Create a manifest specifying the main class
                sh '''
                    echo "Main-Class: largestno" > manifest.txt
                    jar cfm largestno.jar manifest.txt largestno.class
                '''
            }
        }

        stage('Archive JAR') {
            steps {
                // Archive the JAR so you can download it from Jenkins
                archiveArtifacts artifacts: 'largestno.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ JAR created successfully! Check Build Artifacts.'
        }
        failure {
            echo '❌ Build failed! Check console output.'
        }
    }
}
