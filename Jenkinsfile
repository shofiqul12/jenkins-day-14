pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }

    post {
        success {
            emailext(
                subject: "Build SUCCESS: ${env.JOB_NAME}",
                body: "Job ${env.JOB_NAME} build #${env.BUILD_NUMBER} was successful.",
                to: "your-email@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "Build FAILURE: ${env.JOB_NAME}",
                body: "Job ${env.JOB_NAME} build #${env.BUILD_NUMBER} failed.",
                to: "your-email@gmail.com"
            )
        }
    }
}
