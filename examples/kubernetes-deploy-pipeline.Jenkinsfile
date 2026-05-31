@Library('jenkinslib') _

pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 60, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '20'))
    }

    parameters {
        string(name: 'NAMESPACE', defaultValue: 'default', description: 'Kubernetes namespace')
        string(name: 'DEPLOYMENT_NAME', defaultValue: 'demo-service', description: 'Kubernetes deployment name')
        string(name: 'CONTAINER_NAME', defaultValue: 'demo-service', description: 'Container name in deployment')
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag')
    }

    environment {
        IMAGE_REGISTRY = 'registry.example.com'
        IMAGE_REPOSITORY = "${IMAGE_REGISTRY}/${params.DEPLOYMENT_NAME}"
    }

    stages {
        stage('Validate Parameters') {
            steps {
                script {
                    if (!params.NAMESPACE?.trim()) {
                        error 'NAMESPACE is required'
                    }

                    if (!params.DEPLOYMENT_NAME?.trim()) {
                        error 'DEPLOYMENT_NAME is required'
                    }

                    if (!params.CONTAINER_NAME?.trim()) {
                        error 'CONTAINER_NAME is required'
                    }

                    if (!params.IMAGE_TAG?.trim()) {
                        error 'IMAGE_TAG is required'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    kubectl -n ${params.NAMESPACE} set image deployment/${params.DEPLOYMENT_NAME} \
                      ${params.CONTAINER_NAME}=${IMAGE_REPOSITORY}:${params.IMAGE_TAG}

                    kubectl -n ${params.NAMESPACE} rollout status deployment/${params.DEPLOYMENT_NAME} --timeout=300s
                """
            }
        }

        stage('Verify') {
            steps {
                sh """
                    kubectl -n ${params.NAMESPACE} get deployment ${params.DEPLOYMENT_NAME}
                    kubectl -n ${params.NAMESPACE} get pods -l app=${params.DEPLOYMENT_NAME} -o wide || true
                """
            }
        }
    }

    post {
        success {
            echo "Deployment succeeded: ${params.NAMESPACE}/${params.DEPLOYMENT_NAME}:${params.IMAGE_TAG}"
        }

        failure {
            echo "Deployment failed: ${params.NAMESPACE}/${params.DEPLOYMENT_NAME}:${params.IMAGE_TAG}"
        }
    }
}
