# Homelab Cloud Native

This project aims to build a fully cloud-native homelab environment utilizing a dedicated NAS for storage and a personal home router for networking. The setup will leverage modern technologies to ensure efficient deployment, management, and scaling of services.

## Project Structure

- **docs/**: Contains documentation files.
  - **requirement.md**: Detailed description of core project requirements.
  - **architecture.md**: Overview of the project's architecture design.
  
- **manifests/**: Holds Kubernetes resource manifests.
  - **base/**: Base manifests defining essential services and deployments.
  - **overlays/**: Environment-specific overlays for configuration adjustments.

- **scripts/**: Automation scripts for deployment, management, and maintenance tasks.

- **.gitignore**: Specifies files and directories to be ignored by version control.

## Core Requirements

1. **Secret Management**: All cloud-native services will utilize a secret manager to handle sensitive information. An online editing service with mobile client support will be implemented for managing secrets.

2. **GitOps Principles**: The project will adhere to GitOps principles, allowing for code-based management of service deployments and configurations. This approach facilitates easy reconstruction of the entire cluster when necessary.

3. **Blue-Green Deployment**: The architecture will support blue-green deployment strategies, enabling testing of new versions without impacting production services, thereby minimizing the risk of downtime.

## Getting Started

To set up the homelab environment, follow the instructions in the `docs/requirement.md` and `docs/architecture.md` files for detailed requirements and architectural guidelines.

## Contribution

Contributions are welcome! Please refer to the project's documentation for guidelines on how to contribute effectively.