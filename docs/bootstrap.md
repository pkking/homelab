# 总体
要初始化一个新的k8s集群，需要几个步骤
- [安装k8s](https://docs.k3s.io/quick-start)
- [安装argoCD](#在干净的-kubernetes-集群中安装-argo-cd)
- [安装OLM](https://github.com/operator-framework/operator-lifecycle-manager/blob/master/doc/install/install.md#install-with-operator-sdk-olm-install)

# 在干净的 Kubernetes 集群中安装 Argo CD

本文档描述了如何在一个干净的 Kubernetes 集群中安装和配置 Argo CD。

## 前提条件

- 一个干净的 Kubernetes 集群（版本 1.22+）
- `kubectl` 命令行工具已安装并配置好
- 对集群具有管理员权限

## 安装步骤

### 安装 Argo CD Autopilot

这里我使用的linux下的[安装方法](https://argocd-autopilot.readthedocs.io/en/stable/Installation-Guide/#linux-and-wsl-using-curl)

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### bootstrap Argo CD

See [Autopilot documentation](https://argocd-autopilot.readthedocs.io/en/stable/Getting-Started/#bootstrap-argo-cd)

### 配置访问方式

在安装了 k3s 并使用 Traefik Ingress 的环境中，我们推荐将 Argo CD 服务类型修改为 LoadBalancer，这样 Traefik 会自动将服务暴露出来。

## 卸载 Argo CD

如果您需要卸载 Argo CD，可以使用以下命令：

```bash
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl delete namespace argocd