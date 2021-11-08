# Helm 설치

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

# Nginx Ingress 설치


```bash
kubectl create ns ctrl

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm fetch --untar ingress-nginx/ingress-nginx
vim ingress-nginx/values.yaml
# -----------
#  hostPort:
#    enabled: false --> true

# type: LoadBalancer --> ClusterIP

#  nodeSelector:
#    kubernetes.io/os: linux
#    kubernetes.io/hostname: node1  --> 추가
# -----------


helm install ingress-nginx ./ingress-nginx -nctrl
```
