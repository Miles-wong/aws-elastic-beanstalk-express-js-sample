pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'node:16'
        APP_NAME = 'aws-elastic-beanstalk-express-js-sample'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build & Test') {
            agent {
                docker {
                    image "${DOCKER_IMAGE}"
                }
            }
            steps {
                sh 'npm install'
                // No lint or test script defined in package.json, so only build steps are included
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    def dockerImage = docker.build("${APP_NAME}:latest")
                }
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/node_modules/**', allowEmptyArchive: true
            }
        }

        stage('Approval') {
            steps {
                input(message: 'Proceed to deploy?', ok: 'Yes')
            }
        }

        // Add additional deployment stages or other stages here...

    }

    post {
        always {
            notifyBuild('Email or Slack')
        }
    }
}

def notifyBuild(String channel) {
    // Logic to send notifications. This is a placeholder.
    echo "Sending notifications to ${channel}..."
}
