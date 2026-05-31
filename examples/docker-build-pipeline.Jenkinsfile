@Library('jenkinslib') _

pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 45, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '20'))
    }

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: '', description: 'Optional image tag. Default: Jenkins build number')
    }

    environment {
        APP_NAME = 'demo-service'
        IMAGE_REGISTRY = 'registry.example.com'
        RESOLVED_IMAGE_TAG = "${params.IMAGE_TAG ?: env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package -Dmaven.test.skip=true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build \
                      -t ${IMAGE_REGISTRY}/${APP_NAME}:${RESOLVED_IMAGE_TAG} \
                      -t ${IMAGE_REGISTRY}/${APP_NAME}:latest \
                      .
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                    docker push ${IMAGE_REGISTRY}/${APP_NAME}:${RESOLVED_IMAGE_TAG}
                    docker push ${IMAGE_REGISTRY}/${APP_NAME}:latest
                """
            }
        }
    }

    post {
        success {
            echo "Image pushed: ${IMAGE_REGISTRY}/${APP_NAME}:${RESOLVED_IMAGE_TAG}"
        }

        failure {
            echo "Docker pipeline failed: ${APP_NAME}"
        }

        always {
            cleanWs()
        }
    }
}
