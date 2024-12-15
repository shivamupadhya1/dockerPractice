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
	stage('Nexus Deploy') {
            steps {
		dir('ckyc-client') {
		    script{
			echo 'Deploying....'
			def mavenPom = readMavenPom file: 'pom.xml'
			nexusArtifactUploader(
				nexusVersion: 'nexus3',
				protocol: 'http',
				nexusUrl: 'localhost:8089',
				groupId: "${mavenPom.groupId}",
				version: "${mavenPom.version}",
				repository: 'INTEGRATIONS_CLIENT',
				credentialsId: 'NEXUS_CRED',
				artifacts: [
					[artifactId: "${mavenPom.artifactId}",
					 classifier: '',
					 file: "./target/${mavenPom.artifactId}-${mavenPom.version}.jar",
					 type: 'jar'],
					 [artifactId: "${mavenPom.artifactId}",
					  classifier: '',
					  file: "pom.xml",
					  type: "pom"]
				]
			 )
		     }
		}
            }
        }
		
      }		
   }
}
