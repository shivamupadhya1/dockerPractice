pipeline {
    agent any

   

    stages {
        

        stage('Build') {
            steps {
                // Build the project using Maven
                sh 'mvn clean install -Ddependency-check.skip=true'
            }
        }
    }
}
