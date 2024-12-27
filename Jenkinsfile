pipeline {
    agent {
        label 'docker'
    }
    environment {
        GITHUB_REPO = 'https://github.com/bilal-tkxel-do/DevOps-Assignment-6.git' 
        CREDENTIALS_ID = 'github-token'  
        DEPLOY_ENV = 'production'       
        DOCKERHUB_REPO = "bilaltkxel/hotel-app"
        DOCKERHUB_CREDENTIALS = "DOCKERHUB_CREDENTIALS"
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
                steps {
                    script {
                        dockerImage = docker.build DOCKERHUB_REPO + ":$BUILD_NUMBER"
                        }
                }
            }
    
        stage('Deploy our image') {
                steps{
                    script {
                        docker.withRegistry( '', DOCKERHUB_CREDENTIALS ) {
                        dockerImage.push()
                        }
                    }
                }
            }    
       }
    
    post {
        always {
            echo 'This message will always be displayed, regardless of the build result.'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
