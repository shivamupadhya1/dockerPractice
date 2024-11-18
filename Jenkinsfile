pipeline {
    agent any
    environment {
        GIT_URL = 'git@github.com:shivamupadhya1/dockerPractice.git'  // SSH URL of the GitHub repository
        GIT_CREDENTIALS = 'github-ssh-credentials'  // The ID of the Jenkins SSH credentials
        NEXUS_URL = 'http://localhost:8090/repository/INTEGRATION_CLIENTS/'  // Nexus repository URL
       
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
                // Run Maven build
                script {
                    echo 'Building the project with Maven...'
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        stage('Nexus Deploy') {
            steps {
                script {
                    // Upload artifact to Nexus repository
                    echo 'Deploying to Nexus...'
                    def mavenPom = readMavenPom file: 'pom.xml'
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${NEXUS_URL}",
                        groupId: "${mavenPom.groupId}",
                        version: "${mavenPom.version}",
                        repository: '',  // Nexus repository name
                        credentialsId: 'nexus-user',
                        artifacts: [
                            [artifactId: "${mavenPom.artifactId}",
                             classifier: '',
                             file: "./target/${mavenPom.artifactId}-${mavenPom.version}.jar",
                             type: 'jar'],
                            [artifactId: "${mavenPom.artifactId}",
                             classifier: '',
                             file: "pom.xml",
                             type: 'pom']
                        ]
                    )
                }
            }
        }

        

        stage('Test') {
            steps {
                // Run tests (if applicable)
                echo 'Running tests...'
                sh 'mvn test'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()  // Clean up workspace
        }
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
