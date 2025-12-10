pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag')
        booleanParam(name: 'PUSH_IMAGE', defaultValue: true, description: 'Push image to Docker Hub?')
    }

    environment {
        IMAGE_NAME = "devopssteps/my-app-15"
    }

    stages {

        stage('Clone Code') {
            steps {
                echo 'Cloning code from Git...'
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                echo "Building Docker image with tag: ${params.IMAGE_TAG}"
                sh """
                    docker build -t ${IMAGE_NAME}:${params.IMAGE_TAG} .
                """
            }
        }

        stage('Push Image') {
            when {
                expression { return params.PUSH_IMAGE }
            }
            steps {
                echo "Pushing Docker image: ${IMAGE_NAME}:${params.IMAGE_TAG}"

                withCredentials([usernamePassword(
                    credentialsId: 'docker-cred',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh """
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push ${IMAGE_NAME}:${params.IMAGE_TAG}
                    """
                }
            }
        }
    }
}
