# Homelab Cloud Native Project Requirements

## Core Requirements

1. **Secret Management**:
   - [x] All sensitive information must be managed using a secret manager. [Done][1]
   - [x] The secret manager should provide an online editing service for secrets. Supported by [bitward sm operator][2]
   - [x] A mobile client must be available to edit secrets on the go. Supported by [bitward sm operator][2]

2. **GitOps Principles**:
   - [x] The deployment and configuration of all services must adhere to GitOps principles.
   - [x] All configurations should be stored in a version-controlled repository to facilitate easy reconstruction of the entire cluster.
   - [x] Changes to the infrastructure and applications should be made through pull requests to ensure traceability and review.

3. **Blue-Green Deployment**:
   - [ ] The system must support blue-green deployment strategies.
   - [ ] This capability should allow for testing new versions of services without impacting production.
   - [ ] Rollback procedures must be in place to quickly revert to the previous version in case of issues.

## Additional Considerations

- Ensure that all components are designed to be cloud-native, leveraging containerization and orchestration.
- The architecture should be scalable to accommodate future growth and additional services.
- Monitoring and logging solutions should be integrated to provide insights into the system's performance and health.
- Documentation should be maintained to provide clear guidance on setup, usage, and troubleshooting for all components.

[1]: https://github.com/pkking/homelab/pull/9
[2]: https://bitwarden.com/help/secrets-manager-kubernetes-operator/