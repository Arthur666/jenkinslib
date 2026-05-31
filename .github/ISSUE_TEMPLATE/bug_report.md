---
name: Bug report
about: Report a problem with this Jenkins Shared Library
title: "[Bug] "
labels: bug
assignees: ""
---

## Description

Please describe the bug clearly.

## Expected behavior

What did you expect to happen?

## Actual behavior

What actually happened?

## Jenkins environment

- Jenkins version:
- Pipeline type:
  - [ ] Declarative Pipeline
  - [ ] Scripted Pipeline
- Jenkins agent OS:
- Java version:
- Shared Library branch/tag:

## Minimal Jenkinsfile example

```groovy
@Library('jenkinslib') _

pipeline {
    agent any

    stages {
        stage('Example') {
            steps {
                echo 'Example'
            }
        }
    }
}
```

## Error logs

```text
Paste relevant Jenkins console output or stack trace here.
```

## Additional context

Add any other context about the problem here.
