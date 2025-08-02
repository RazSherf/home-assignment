# Two-Site Demo on Minikube

This repo shows **how to run two tiny static sites** on a local Kubernetes cluster (Minikube) with a single URL.

| Path | What you’ll see | Namespace |
|------|-----------------|-----------|
| `http://<minikube-IP>/site1` | **Hello Site 1** | `site1` |
| `http://<minikube-IP>/site2` | **Hello Site 2** | `site2` |

---

## How it works

1. **Docker images**  
   Each folder (`site1/`, `site2/`) has:
   * `index.html` – the page content  
   * `Dockerfile` – copies that file into an Nginx container.

2. **Kubernetes objects** (`k8s/*.yaml`)  
   * One **Namespace** per site  
   * A **Deployment + Service** per site  
   * A single **Ingress** that rewrites  
     * `/site1 → service site1`  
     * `/site2 → service site2`

3. **Minikube** runs everything locally; images are built directly inside its Docker daemon, so no external registry is needed.

---

## Quick start

```bash
# 1) Make sure ingress is enabled once
minikube addons enable ingress

# 2) Build images inside Minikube’s Docker
eval $(minikube -p minikube docker-env)
docker build -t site1:latest ./site1
docker build -t site2:latest ./site2

# 3) Deploy everything
kubectl apply -f k8s/namespaces.yaml
kubectl apply -f k8s/deployments.yaml
kubectl apply -f k8s/services.yaml
kubectl apply -f k8s/ingress.yaml

# 4) Test
MINI_IP=$(minikube ip)
curl http://$MINI_IP/site1   # → Hello Site 1
curl http://$MINI_IP/site2   # → Hello Site 2
