# Secrets
All the secrets the homelab needed

## bitwarden
This is the only secret that should be created manually, once bitwarden-sm installed, all secret can be managed by [bitwarden secrets manager operator](https://bitwarden.com/help/secrets-manager-kubernetes-operator/#example-usage-chart)

But we need to bootstrap a secret with the access token to allow the operator to fetch secrets from bitwarden vault.

```
kubectl create secret generic bw-auth-token -n <namespace> --from-literal=token="<TOKEN_HERE>"
```

The secret can be decode using the command:

```
kubectl get secret bw-auth-token -n <namespace> -o jsonpath='{.data.token}'|base64 -d
```

NOTE: This secret needs to be created in each namespace that will use the operator, as the `BitwardenSecret` resource is namespace-scoped.

NOTE: You should assign the 'machine account' to a project with read-only access permission which holds the secrets you want to sync to your cluster.

The `TOKEN_HERE` should be the [access token](https://bitwarden.com/help/access-tokens/) created in [machine account](https://bitwarden.com/help/machine-accounts/)

