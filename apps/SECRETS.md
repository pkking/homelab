# Secrets
All the secrets the homelab needed

## bitwarden
```
kubectl create secret generic bw-auth-token -n sm-operator-system --from-literal=token="<TOKEN_HERE>"
```

The `TOKEN_HERE` should be the [access token](https://bitwarden.com/help/access-tokens/) created in [machine account](https://bitwarden.com/help/machine-accounts/)

## synology csi

fill the DSM info
```
# cat dsm-info.yml
apiVersion: v1
kind: Secret
metadata:
  name: synology-csi-client-info
stringData:
  client-info.yaml: |
    clients:
    - host: 192.168.50.225
      https: false
      password: Pkking@123
      port: 5000
      username: pkking
```
create secret
```
kubectl apply -f dsm-info.yaml -n synology-csi-system
```
