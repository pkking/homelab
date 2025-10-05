# argocd applications archived
This dir contains all my cluster argocd applications declarative yamls, inspired by [app of apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/)

the structure looks like:
```
├── Chart.yaml
├── templates
│   ├── guestbook.yaml
│   ├── helm-dependency.yaml
│   ├── helm-guestbook.yaml
│   └── kustomize-guestbook.yaml
└── values.yaml 
```

# usage
Since we are using argocd-autopilot to manage argocd itself, we can use the following command to create an application that points to this repository.

```bash
argocd-autopilot app create apps --app github.com/pkking/homelab/apps-index/ -p default --type helm
```

# bootstrap

Before creating the apps application as described above, you need to bootstrap your cluster with Argo CD.
Follow the instructions in [docs/bootstrap.md](../docs/bootstrap.md) to install Argo CD in your clean Kubernetes cluster.

After successfully installing Argo CD, you can proceed with creating the apps application using the commands above.

** NOTE **:
1. IF WE want to update the `synology-csi storageclass`, need delete original storageclass manually