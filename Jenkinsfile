pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker-compose up -d'
                sh 'docker-compose ps'
            }
        }

        stage('Run Spark Job') {
            steps {
                sh 'docker-compose logs --tail=100'
            }
        }
    }

    post {
        success {
            slackSend channel: '#spark-alerts', color: 'good',
                message: "Build SUCCESSFUL: ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL})"
        }

        failure {
            slackSend channel: '#spark-alerts', color: 'danger',
                message: "Build FAILED: ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL})"
        }

        always {
            sh 'docker-compose down'
        }
    }
}
