## bitwarden
```
kubectl create secret generic bw-auth-token -n sm-operator-system --from-literal=token="<TOKEN_HERE>"
```

## synology csi

fill the DSM info
```
# cat client-info.yaml
clients:
- host: 192.168.1.1
    https: false
    password: changeme
    port: 5000
    username: changeme
- host: 192.168.1.1
    https: true
    password: changeme
    port: 5001
    username: changeme
```
create secret
```
kubectl create secret -n synology-csi-system generic client-info-secret --from-file=dsm-info.yml
```
