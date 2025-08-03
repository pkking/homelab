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