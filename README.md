# jenkinslib

`jenkinslib` is an open-source Jenkins Shared Library designed to help DevOps teams standardize and reuse Jenkins Pipeline logic across multiple services.

The project provides reusable Groovy-based pipeline components, helper functions, and examples for common CI/CD scenarios such as Maven builds, code scanning, Docker image builds, Kubernetes deployments, timeout control, formatted console output, and post-build handling.

## Why this project exists

In many Jenkins environments, teams repeatedly copy similar Pipeline code across repositories. This often leads to inconsistent build logic, difficult maintenance, duplicated Jenkinsfile code, and higher operational risk.

`jenkinslib` aims to solve this by moving reusable CI/CD logic into a centralized Jenkins Shared Library, allowing application repositories to keep their Jenkinsfiles simple, readable, and consistent.

## Key goals

- Reduce duplicated Jenkins Pipeline code.
- Standardize CI/CD workflow implementation across projects.
- Provide reusable Groovy helpers for Jenkins Pipeline stages.
- Improve maintainability of Jenkins-based delivery systems.
- Make Jenkinsfiles easier to read and audit.
- Provide practical examples for Maven, Docker, and Kubernetes delivery workflows.
- Support future integration with security scanning, deployment approval, notification, and release automation.

## Repository structure

```text
jenkinslib/
├── src/
│   └── org/
│       └── devops/
│           └── *.groovy
├── vars/
│   └── *.groovy
├── examples/
│   └── *.Jenkinsfile
├── Jenkinsfile
├── README.md
├── ROADMAP.md
└── LICENSE
```

### `src/org/devops`

Contains reusable Groovy classes and helper logic.

### `vars`

Contains Jenkins Shared Library global variables and pipeline steps that can be called directly from Jenkinsfiles.

### `examples`

Contains practical Jenkinsfile examples showing how to use this shared library in real CI/CD workflows.

## How to use this library in Jenkins

### 1. Configure Jenkins Shared Library

In Jenkins:

```text
Manage Jenkins
  -> System
  -> Global Pipeline Libraries
  -> Add
```

Recommended configuration:

```text
Name: jenkinslib
Default version: master
Retrieval method: Modern SCM
Source Code Management: Git
Project Repository: https://github.com/Arthur666/jenkinslib.git
```

You can also pin the library to a specific tag or branch for production stability.

Example:

```text
Default version: v0.1.0
```

### 2. Import the library in Jenkinsfile

```groovy
@Library('jenkinslib') _
```

### 3. Use shared steps or helper functions

Example:

```groovy
@Library('jenkinslib') _

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    println "Using Jenkins Shared Library: jenkinslib"
                }
            }
        }
    }
}
```

## Example: basic Maven pipeline

```groovy
@Library('jenkinslib') _

pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
    }

    environment {
        APP_NAME = 'demo-service'
        MAVEN_OPTS = '-Dmaven.test.skip=true'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -Dmaven.test.skip=true'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Build succeeded: ${env.APP_NAME}"
        }

        failure {
            echo "Build failed: ${env.APP_NAME}"
        }

        always {
            cleanWs()
        }
    }
}
```

## Example: Docker build pipeline

```groovy
@Library('jenkinslib') _

pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 45, unit: 'MINUTES')
    }

    environment {
        APP_NAME = 'demo-service'
        IMAGE_REGISTRY = 'registry.example.com'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
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
                      -t ${IMAGE_REGISTRY}/${APP_NAME}:${IMAGE_TAG} \
                      -t ${IMAGE_REGISTRY}/${APP_NAME}:latest \
                      .
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                    docker push ${IMAGE_REGISTRY}/${APP_NAME}:${IMAGE_TAG}
                    docker push ${IMAGE_REGISTRY}/${APP_NAME}:latest
                """
            }
        }
    }

    post {
        success {
            echo "Docker image pushed: ${IMAGE_REGISTRY}/${APP_NAME}:${IMAGE_TAG}"
        }

        failure {
            echo "Docker image build or push failed"
        }

        always {
            cleanWs()
        }
    }
}
```

## Example: Kubernetes deployment pipeline

```groovy
@Library('jenkinslib') _

pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 60, unit: 'MINUTES')
    }

    parameters {
        string(name: 'NAMESPACE', defaultValue: 'default', description: 'Kubernetes namespace')
        string(name: 'DEPLOYMENT_NAME', defaultValue: 'demo-service', description: 'Kubernetes deployment name')
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag')
    }

    environment {
        IMAGE_REGISTRY = 'registry.example.com'
        APP_NAME = "${params.DEPLOYMENT_NAME}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                    kubectl -n ${params.NAMESPACE} set image deployment/${params.DEPLOYMENT_NAME} \
                      ${params.DEPLOYMENT_NAME}=${IMAGE_REGISTRY}/${APP_NAME}:${params.IMAGE_TAG}

                    kubectl -n ${params.NAMESPACE} rollout status deployment/${params.DEPLOYMENT_NAME} --timeout=300s
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
```

## Planned capabilities

The project is under active development. Planned improvements include:

- More reusable Jenkins Pipeline global variables.
- Standardized Maven build helper.
- Docker build and push helper.
- Kubernetes deployment helper.
- Helm deployment helper.
- SonarQube integration.
- Trivy or other container image scanning integration.
- Git branch and tag strategy helpers.
- Notification integration for Feishu, Slack, or email.
- Release tagging workflow.
- Better test coverage for Groovy shared library code.
- More complete documentation and real-world examples.

See [ROADMAP.md](./ROADMAP.md) for details.

## Contributing

Contributions are welcome.

You can contribute by:

- Reporting bugs.
- Suggesting new reusable pipeline steps.
- Improving documentation.
- Adding Jenkinsfile examples.
- Refactoring Groovy code.
- Adding tests.
- Improving CI/CD security practices.

Before submitting a pull request, please make sure:

- The code is readable and maintainable.
- The Jenkins Pipeline syntax is valid.
- Examples are tested or clearly marked as reference examples.
- Documentation is updated when behavior changes.

## License

This project is licensed under the MIT License.

See [LICENSE](./LICENSE) for details.
