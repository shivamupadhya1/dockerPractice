pipeline {
    

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                git branch: 'main', url: 'https://github.com/shivamupadhya1/dockerPractice.git' // Update with your repo URL
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
