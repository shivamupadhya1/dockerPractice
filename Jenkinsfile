pipeline {
    agent any
    environment {
        GIT_URL = 'https://github.com/shivamupadhya1/dockerPractice.git'  // Define URL as an environment variable
        GIT_CREDENTIALS_ID = 'GitHub-Access'  // Define the credentials ID as an environment variable
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout from GitHub using HTTPS and credentials stored in Jenkins
                git(
                    url: "${env.GIT_URL}",  // Use the environment variable for the GitHub repo URL
                    credentialsId: "${env.GIT_CREDENTIALS_ID}"  // Use the environment variable for the credentials ID
                )
            }
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
                        nexusUrl: 'http://localhost:8090/',
                        groupId: "${mavenPom.groupId}",
                        version: "${mavenPom.version}",
                        repository: 'INTEGRATION_CLIENTS',  // Nexus repository name
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
