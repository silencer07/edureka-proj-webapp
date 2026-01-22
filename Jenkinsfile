pipeline {
	agent any
	stages {
		// stage('Pull from GitHub') {
		//     steps {
		//         git branch: 'main',
		//             url: 'https://github.com/silencer07/edureka-proj-webapp'
		//     }
		// }
		stage('Build') {
			steps {
				// Run Maven on a Unix agent.
				sh "mvn clean test"
			}

			post {
				success {
					junit '**/target/surefire-reports/TEST-*.xml'
				}
			}
		}

		stage('Execute tests') {
			steps {
				sh "mvn package -DskipTests"
				sh "ls -lah"
			}

			post {
				success {
					archiveArtifacts '**/target/*.war'
				}
			}
		}
	}
}
