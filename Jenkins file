pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { git branch: 'master', url: '<your-repo-url>' }
        }
        stage('Build') { steps { sh 'echo Building project...' } }
        stage('Test')  { steps { sh 'echo Running tests...' } }
        stage('Deploy'){ steps { sh 'echo Deploying project...' } }
    }
}
