pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { git branch: 'main', url: 'https://github.com/Dinesh-SMG/Learning-Experiments.git' }
        }
        stage('Build') { steps { sh 'echo Building project...' } }
        stage('Test')  { steps { sh 'echo Running tests...' } }
        stage('Deploy'){ steps { sh 'echo Deploying project...' } }
    }
}
