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

### 1. 创建 Argo CD 命名空间

```bash
kubectl create namespace argocd
```

### 2. 安装 Argo CD

使用官方的安装清单来部署 Argo CD：

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

或者，如果您希望安装最新版本（可能包含实验性功能）：

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/master/manifests/install.yaml
```

### 3. 等待组件启动

等待所有组件启动完成，这可能需要几分钟时间：

```bash
kubectl wait --for=condition=available --timeout=600s deployment/argocd-server -n argocd
```

### 4. 配置访问方式

在安装了 k3s 并使用 Traefik Ingress 的环境中，我们推荐将 Argo CD 服务类型修改为 LoadBalancer，这样 Traefik 会自动将服务暴露出来。

#### 方式一：修改服务类型为 LoadBalancer 并暴露端口 8081

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer", "ports": [{"name": "https", "port": 8081, "protocol": "TCP", "targetPort": "8080"}]}}'
```

由于使用了 k3s 的 Traefik Ingress，服务会自动暴露，您可以通过以下方式获取访问地址：

```bash
kubectl get svc argocd-server -n argocd
```

#### 方式二：端口转发（临时访问）

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

然后在浏览器中访问 `https://localhost:8080`

#### 方式三：通过 Ingress 访问（可选）

如果您希望使用自定义域名访问 Argo CD UI，可以创建一个 Ingress 资源：

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: argocd-server-ingress
  namespace: argocd
  annotations:
    traefik.ingress.kubernetes.io/router.tls: "true"
spec:
  rules:
  - host: argocd.example.com  # 替换为您的域名
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: argocd-server
            port:
              name: https
```

### 5. 获取初始密码

Argo CD 安装后会创建一个默认的管理员账户 `admin`，密码是自动生成的，可以通过以下命令获取：

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### 6. 登录 Argo CD

使用 `admin` 用户和上一步获取的密码登录 Argo CD UI。

如果您使用的是 LoadBalancer 方式，访问地址是 `https://<EXTERNAL-IP>`。
如果您使用的是端口转发方式，访问地址是 `https://localhost:8080`。

您也可以使用 Argo CD CLI 登录：

```bash
argocd login <ARGOCD_SERVER> --username admin --password <PASSWORD>
```

### 7. 修改默认密码（推荐）

为了安全起见，建议您在首次登录后修改默认密码：

```bash
argocd account update-password
```

## 验证安装

安装完成后，您可以通过以下方式验证 Argo CD 是否正常工作：

1. 检查所有 Pod 是否处于 Running 状态：
   ```bash
   kubectl get pods -n argocd
   ```

2. 检查所有服务是否正常：
   ```bash
   kubectl get svc -n argocd
   ```

3. 访问 Argo CD UI 并登录

## 后续步骤

安装完成后，您可以：

1. 配置 Git 仓库以进行应用部署
2. 创建应用并将其同步到集群
3. 配置 RBAC 和 SSO（可选）
4. 配置通知和监控（可选）

## 故障排除

如果遇到问题，请检查以下几点：

1. 确保所有 Pod 都处于 Running 状态
2. 检查服务是否正确暴露
3. 查看 Pod 日志以获取更多信息：
   ```bash
   kubectl logs -n argocd <pod-name>
   ```

## 卸载 Argo CD

如果您需要卸载 Argo CD，可以使用以下命令：

```bash
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl delete namespace argocd