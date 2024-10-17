pipeline {
    agent any
    tools {
        maven 'Maven 3.8.6' // Ensure this matches the name in Jenkins Global Tool Configuration
    }// Specifies that the pipeline can run on any available agent

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                git branch: 'main', url: 'https://github.com/shivamupadhya1/dockerPractice.git'
            }
        }

        stage('Build') {
            steps {
                // Build the project using Maven
                script {
                    sh 'mvn clean install -Ddependency-check.skip=true'
                }
            }
        }
    }
}
