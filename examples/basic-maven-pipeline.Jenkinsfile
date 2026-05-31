@Library('jenkinslib') _

pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '20'))
    }

    environment {
        APP_NAME = 'demo-service'
        MAVEN_ARGS = '-Dmaven.test.skip=true'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package ${MAVEN_ARGS}"
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Build succeeded: ${APP_NAME}"
        }

        failure {
            echo "Build failed: ${APP_NAME}"
        }

        always {
            cleanWs()
        }
    }
}
