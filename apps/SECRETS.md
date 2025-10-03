# Secrets
All the secrets the homelab needed

## bitwarden
This is the only secret should be created manually, once bitwarden-sm installed, all secret can be managed by [bitwarden secrets manager operator](https://bitwarden.com/help/secrets-manager-kubernetes-operator/#example-usage-chart)
```
kubectl create secret generic bw-auth-token -n sm-operator-system --from-literal=token="<TOKEN_HERE>"
```

The `TOKEN_HERE` should be the [access token](https://bitwarden.com/help/access-tokens/) created in [machine account](https://bitwarden.com/help/machine-accounts/)

