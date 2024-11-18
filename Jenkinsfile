pipeline {
    agent any
    environment {
        GIT_URL = 'git@github.com:your-username/your-repository.git'  // SSH URL of the GitHub repository
        GIT_CREDENTIALS = 'github-ssh-credentials'  // The ID of the Jenkins credential you created
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout from GitHub using SSH
                git credentialsId: "${GIT_CREDENTIALS}", url: "${GIT_URL}"
            }
        }
        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'mvn clean install'
            }
        }
    }
}
