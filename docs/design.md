# Design descision
## Problems
When designing the system, we faced several challenges:
1. **Secret Management**: Ensuring that sensitive information such as API keys, passwords, and tokens are securely managed and easily accessible.
2. **GitOps Principles**: Implementing a deployment strategy that adheres to GitOps principles, allowing for version-controlled configurations and easy rollbacks.

## Solutions
### Secret Management
#### Why not use kubernetes secret
The kubernetes secret is base64 encoded, which is not secure enough for sensitive information. To enhance security, we decided to use a dedicated secret manager that provides encryption and access control features. This ensures that sensitive data is stored securely and can only be accessed by authorized applications.

#### Why bitward secret manager
We chose the Bitwarden SM Operator for secret management because it offers [kubernetes integration][1], allowing secrets to be injected into pods at runtime. Additionally, it provides a web console for online editing of secrets, making it convenient for users to manage sensitive information on the go.

#### future plan
bitwarden sm operator is integrated with [external secret operator](https://external-secrets.io/), a more generic secret manager operator. In the future, we may switch to other secret managers supported by external secret operator, such as AWS Secrets Manager or HashiCorp Vault, if our requirements change.

### GitOps Principles
To adhere to GitOps principles, we decided to use Argo CD for continuous deployment. Argo CD allows us to store all deployment configurations in a Git repository, enabling version control and easy rollbacks. Whenever changes are pushed to the repository, Argo CD automatically deploys the updates to the Kubernetes cluster, ensuring that the cluster state matches the desired state defined in Git.

### Which kind of application should be deployed using GitOps
- For stateless applications, we deploy them using [Helm](https://helm.sh/) and [Argo CD](https://argo-cd.readthedocs.io/en/stable/). This combination allows us to manage application configurations effectively and leverage the benefits of both tools. See [apps/README.md](../apps/README.md) for more details.
- For stateful applications, we deploy them using [Operator Lifecycle Manager (OLM)](https://olm.operatorframework.io/). OLM provides a robust framework for managing the lifecycle of operators, making it suitable for stateful applications that require more complex management. See [operators/README.md](../operators/README.md) for more details.
- For CRDs, we use argo cd to deploy them, so that we can manage the version of CRDs using git. See [crds/README.md](../crds/README.md) for more details.

### Combine operator and crds to deploy stateful applications
For stateful applications, we use a combination of operators and Custom Resource Definitions (CRDs)
    to manage their deployment and lifecycle. Operators provide a way to automate the management of complex applications, while CRDs allow us to define custom resources that represent the desired state of these applications.
    
[1]: https://bitwarden.com/help/secrets-manager-kubernetes-operator/