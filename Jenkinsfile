pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
				dir('ckyc-client') {
					echo 'Building..'
					lastChanges since: 'LAST_SUCCESSFUL_BUILD', format:'SIDE',matching: 'LINE'
					sh 'mvn clean install -Ddependency-check.skip=true'
				}                
            }
        }
		
		
		stage('OWASP-Dep-Check-Report') {
            steps {
				dir('integration_clients') {
				script {	
				//	dependencyCheck additionalArguments: ''' 
				//				-o './report'
				//				-s './target'
				//				-f 'ALL' 
				//				--prettyPrint''', odcInstallation: 'OWASP-Dependency-Check'
					try { sh 'mvn verify' } catch (Exception e) { echo 'Exception occurred: ' + e.toString()  }					
				//	dependencyCheckPublisher pattern: './report/dependency-check-report.xml'
                }
				}
			}
		}
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Nexus Deploy') {
            steps {
				dir('integration_clients') {
				    script{
						echo 'Deploying....'
						def mavenPom = readMavenPom file: 'pom.xml'
						nexusArtifactUploader(
							nexusVersion: 'nexus3',
							protocol: 'http',
							nexusUrl: '192.168.156.51:8081',
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
		stage('SonarQube Analysis') {
			steps {
				script {
					// Navigate to the directory containing the Maven project
					dir('integration_clients') {
						// Use the SonarQube scanner with environment variables
						withSonarQubeEnv(installationName: 'Sonar-server', credentialsId: 'SONAR_TOKEN') {
							// Read the Maven POM file
							def pom = readMavenPom file: "pom.xml"
							// Define the SonarQube scanner home
							def scannerHome = tool 'sonar-scanner'
							// Define the location of compiled classes
							def compiledClassesDir = 'target/classes'
							// Execute the SonarQube scanner command with sonar.java.binaries property
							sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=${pom.groupId}:${pom.artifactId} -Dsonar.sources=src -Dsonar.java.binaries=${compiledClassesDir} -Dsvnkit.http.methods=Basic,Digest,Negotiate,NTLM"
							sh 'mvn sonar:sonar	-Dsonar.scm.disabled=True'
						}
					}
				}
			}
		}
	}
			post {
		always {
			script {
				sh 'pwd'
			}
			dependencyCheckPublisher pattern: 'integration_clients/target/report/dependency-check-report.xml'
			
			}
		}
}
