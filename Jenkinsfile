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
		
		stage('Docker Build and Upload to registry') {
			agent {
				kubernetes {
					label 'dind-agent' // Label for the Kubernetes agent with Docker support
				}
			}
			steps {
				container('dind-agent') {
				    checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/StephenGrider/docker-react.git']]])
                    script {
                            echo "Bulding docker images"
                        docker.build(
                            "asikrasool/reactapp:latest")
                    }
			    }
			}
		}
	}
}