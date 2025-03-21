pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building the application..."'
                sh 'docker build -t my-app .'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=my-app -Dsonar.sources=.'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying the application..."'
                sh 'docker run -d --name my-app -p 80:80 my-app'
            }
        }
    }
}
