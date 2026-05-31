# Roadmap

This roadmap describes the planned development direction of `jenkinslib`.

The goal of this project is to evolve from a personal Jenkins Shared Library into a reusable open-source CI/CD automation toolkit for Jenkins-based DevOps teams.

## v0.1.0 - Project foundation

- [x] Initialize Jenkins Shared Library repository.
- [x] Add basic Groovy shared library structure.
- [x] Add README documentation.
- [x] Add MIT license.
- [x] Add basic Jenkinsfile examples.
- [x] Add issue and pull request templates.

## v0.2.0 - Build workflow helpers

- [ ] Add reusable Maven build helper.
- [ ] Add reusable Gradle build helper.
- [ ] Add build timeout wrapper.
- [ ] Add formatted console output helpers.
- [ ] Add artifact archive helper.
- [ ] Add post-build cleanup helper.

## v0.3.0 - Docker workflow helpers

- [ ] Add Docker image build helper.
- [ ] Add Docker image tag strategy helper.
- [ ] Add Docker image push helper.
- [ ] Add image registry authentication helper.
- [ ] Add container image metadata helper.

## v0.4.0 - Kubernetes deployment helpers

- [ ] Add Kubernetes deployment helper.
- [ ] Add rollout status check helper.
- [ ] Add rollback helper.
- [ ] Add namespace validation helper.
- [ ] Add deployment image update helper.
- [ ] Add deployment health check helper.

## v0.5.0 - Helm deployment helpers

- [ ] Add Helm upgrade/install helper.
- [ ] Add Helm rollback helper.
- [ ] Add Helm values validation helper.
- [ ] Add Helm release history helper.
- [ ] Add support for environment-specific values files.

## v0.6.0 - Security and quality checks

- [ ] Add SonarQube scan helper.
- [ ] Add Trivy image scan helper.
- [ ] Add dependency vulnerability scan helper.
- [ ] Add secret scanning helper.
- [ ] Add security gate support before deployment.

## v0.7.0 - Notification integrations

- [ ] Add Feishu notification helper.
- [ ] Add Slack notification helper.
- [ ] Add email notification helper.
- [ ] Add build result summary helper.
- [ ] Add deployment result summary helper.

## v1.0.0 - Stable release

- [ ] Provide stable public API for shared steps.
- [ ] Add complete usage documentation.
- [ ] Add more real-world Jenkinsfile examples.
- [ ] Add automated tests where possible.
- [ ] Add release notes.
- [ ] Publish the first stable release tag.
