pipeline {
    agent none // 初始阶段不使用docker agent
    stages {
        stage('Prepare Environment') {
            agent any
            steps {
                script {
                    def img = docker.image('node:16')
                    try {
                        img.inspect()
                    } catch (Exception) {
                        img.pull()
                    }
                }
            }
        }
        stage('Build') {
            agent {
                docker {
                    image 'node:16'
                }
            }
            steps {
                sh 'npm install --save'
            }
        }
    }
}

