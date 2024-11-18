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
        stage('Upload to Nexus') {
            steps {
                script {
                    def mavenPom = readMavenPom file: 'pom.xml'
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${env.NEXUS_URL}",
                        groupId: "${mavenPom.groupId}",
                        version: "${mavenPom.version}",
                        repository: "${env.REPOSITORY}",
                        credentialsId: "${env.NEXUS_CREDENTIALS_ID}",
                        artifacts: [
                            [artifactId: "${mavenPom.artifactId}",
                             classifier: '',
                             file: "target/${mavenPom.artifactId}-${mavenPom.version}.jar",
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
    }
}
