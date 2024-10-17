pipeline {
    agent any
    
        stage('Build') {
            steps {
                // Build the project using Maven
                script {
                    bat 'mvn clean install -Ddependency-check.skip=true'
                }
            }
        }
    }
