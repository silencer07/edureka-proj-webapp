pipeline {
	agent any
	stages {
		stage('Execute tests') {
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

		stage('Package') {
			steps {
				sh "mvn package -DskipTests"
			}

			post {
				success {
					archiveArtifacts '**/target/*.war'
				}
			}
		}
	}
}
