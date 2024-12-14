pipeline {
    agent any
    stages {
        stages {
        stage('Build') {
            steps {
		dir('ckyc-client'){
                echo 'Building the project...'
                bat 'mvn clean install'
		}
            }
        }
	}
		
		
}
}
