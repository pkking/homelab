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
When bootstrap a new argocd, run [argo cli](https://argo-cd.readthedocs.io/en/stable/cli_installation/)
```
argocd login <url to argocd>
argocd app create apps \
    --sync-option auto
    --dest-namespace argocd \
    --dest-server https://kubernetes.default.svc \
    --repo https://github.com/pkking/homelab.git \
    --path apps
```
This will automated sync all apps in cluster right now!

# bootstrap

Before creating the apps application as described above, you need to bootstrap your cluster with Argo CD.
Follow the instructions in [docs/bootstrap.md](../docs/bootstrap.md) to install Argo CD in your clean Kubernetes cluster.

After successfully installing Argo CD, you can proceed with creating the apps application using the commands above.