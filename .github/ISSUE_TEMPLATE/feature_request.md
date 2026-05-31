---
name: Feature request
about: Suggest a new reusable Jenkins Pipeline feature
title: "[Feature] "
labels: enhancement
assignees: ""
---

## Feature description

Please describe the feature you would like to add.

## Use case

What CI/CD problem does this feature solve?

## Proposed usage

Please provide an example of how this feature might be used in a Jenkinsfile.

```groovy
@Library('jenkinslib') _

pipeline {
    agent any

    stages {
        stage('Example') {
            steps {
                script {
                    // proposed usage
                }
            }
        }
    }
}
```

## Alternatives considered

Have you considered any alternative implementation?

## Additional context

Add any other context or screenshots here.
