# Two-Site Demo on Minikube 🚀

Spin up **two tiny static sites** on your local Kubernetes (Minikube) in minutes.

| URL | What you’ll see | Namespace |
|-----|-----------------|-----------|
| `http://<minikube-IP>/site1` | **Hello Site 1** 🌟 | `site1` |
| `http://<minikube-IP>/site2` | **Hello Site 2** 🌟 | `site2` |

---

## How it works 🤔

1. **Docker images** 🐳  
   Each folder (`site1/`, `site2/`) has an `index.html` and a one-line `Dockerfile` that copies it into Nginx.

2. **Kubernetes objects** ☸️  
   * One **Namespace**, **Deployment** and **Service** per site  
   * A single **Ingress** that routes  
     * `/site1` ➡️ Site 1  
     * `/site2` ➡️ Site 2  

3. **Minikube** 🖥️  
   Images are built inside Minikube’s Docker daemon—no external registry needed.

---

## Quick start ⚡

```bash
# 1) Enable ingress once
minikube addons enable ingress   # ⚙️

# 2) Point Docker to Minikube
eval $(minikube -p minikube docker-env)

# 3) Build images 🐳
docker build -t site1:latest ./site1
docker build -t site2:latest ./site2

# 4) Deploy objects ☸️
kubectl apply -f k8s/namespaces.yaml
kubectl apply -f k8s/deployments.yaml
kubectl apply -f k8s/services.yaml
kubectl apply -f k8s/ingress.yaml

# 5) Test 🚀
MINI_IP=$(minikube ip)
curl http://$MINI_IP/site1   # → Hello Site 1
curl http://$MINI_IP/site2   # → Hello Site 2
