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
				// dir('build/'){
				// 	pwd();
				// 	withAWS(region:'us-east-1',credentials:'jenkins_manual') {
				// 		s3Upload(bucket:"scp-demo-ou", path:'build/', workingDir:'/', includePathPattern:'**/*');
				// 	}
				// };
				withAWS(region:'us-east-1',credentials:'jenkins_manual') {
				sh 'tar -czf /build/artifacts.tar.gz build/'
					s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'build/artifacts.tar.gz', bucket:'scp-demo-ou')
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