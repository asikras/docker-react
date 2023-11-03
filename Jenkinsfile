pipeline {
	agent {
		kubernetes {
				inheritFrom 'nodejs'
		}
	}

	stages {
		stage('Checkout') {
			steps {
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/StephenGrider/docker-react.git']]])
			}
		}

		stage('Build') {
            steps {
                script {
                    sh 'npm install'
                }
            }
		}

		stage('Test') {
            steps {
                script {
                    sh 'npm test'
                }
            }
		}

		stage('Build Production') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
		}
		stage('Upload') {
			steps {
				dir('build/'){
					// pwd();
					withAWS(region:'us-east-1',credentials:'jenkins_manual') {
						s3Upload(bucket:"scp-demo-ou", path:'build/, workingDir:'/', includePathPattern:'**/*')
					}
				}
    		}
		}
	post {
		always {
			archiveArtifacts artifacts: 'build/'
		}
    }
}