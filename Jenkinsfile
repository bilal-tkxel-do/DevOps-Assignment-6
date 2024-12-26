pipeline {
    agent any
    environment {
        GITHUB_REPO = 'https://github.com/bilal-tkxel-do/DevOps-Assignment-4.git' 
        CREDENTIALS_ID = 'github-token'  
        DEPLOY_ENV = 'production'       
        DOCKER_IMAGE = 'test-app:latest'                 
    }
  
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', 
                    credentialsId: "${CREDENTIALS_ID}",
                    url: "${GITHUB_REPO}"
            }
        }

	stage('Code Analysis') {
	    agent {
                label 'docker'  
            }

            environment {
                scannerHome = tool name: 'sonar'
            }
            steps {
                script {
                    withSonarQubeEnv('sonar') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=devops-assignment-6 \
                                -Dsonar.projectName=devops-assignment-6 \
                                -Dsonar.sources=.
                        """
                    }
                }
            }
        }
	
	stage('Build Docker Image') {
	    agent {
                label 'docker'
            }

            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }
    }
}
