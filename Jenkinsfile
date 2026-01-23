pipeline {
	agent any

	environment {
	    DOCKERHUB_USER = 'silencer07'
        DOCKERHUB_REPO = 'edureka-proj-webapp'
        BUILD_NUMBER_TAG = "${env.BUILD_NUMBER}"
    }

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

		stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        whoami
                        groups
                        ls -l /var/run/docker.sock
                    """
                }

                script {
                    sh """
                        docker buildx build \
                          --platform linux/amd64 \
                          -t $DOCKERHUB_USER/$DOCKERHUB_REPO:$BUILD_NUMBER_TAG \
                          --load .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_TOKEN'
                )]) {
                    sh """
                      echo "$DOCKERHUB_TOKEN" | docker login \
                        -u "$DOCKERHUB_USER" \
                        --password-stdin
                    """
                }

                script {
                    sh """
                        docker push $DOCKERHUB_USER/$DOCKERHUB_REPO:$BUILD_NUMBER_TAG
                    """
                }
            }
        }
	}
}
