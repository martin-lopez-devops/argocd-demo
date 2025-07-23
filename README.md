# ArgoCD GitOps Demo (Local POC with kind)

This repository provides a minimal GitOps proof-of-concept using [ArgoCD](https://argo-cd.readthedocs.io/) on a local Kubernetes cluster with [kind](https://kind.sigs.k8s.io/). It showcases the basic GitOps flow: you declare your application configuration in Git, and ArgoCD continuously reconciles the desired state with the actual state in the cluster.

---

## 📦 Requirements

* [Docker](https://www.docker.com/) installed and running
* [kubectl](https://kubernetes.io/docs/tasks/tools/) installed
* [kind](https://kind.sigs.k8s.io/) installed
* [ArgoCD CLI (optional)](https://argo-cd.readthedocs.io/en/stable/cli_installation/)
* [Git](https://git-scm.com/) configured and GitHub account available

---

## 🚀 Setup Instructions

### 1. Clone this repository

```bash
git clone https://github.com/martin-lopez-devops/argocd-demo.git
cd argocd-demo
```

### 2. Create a local Kubernetes cluster using kind

```bash
kind create cluster --name argocd-demo
```

### 3. Install ArgoCD in the cluster

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 4. Expose ArgoCD UI locally (port-forward)

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Access ArgoCD at: [https://localhost:8080](https://localhost:8080)

### 5. Get admin password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Log in using username `admin` and the password retrieved above.

### 6. Deploy sample application

```bash
kubectl apply -f app.yaml
```

> This defines an ArgoCD Application resource pointing to this same GitHub repo. It will deploy the demo app in `default` namespace.

---

## 📁 Repository Structure

```
argocd-demo/
├── app/
│   ├── deployment.yaml   # Sample NGINX deployment
│   └── service.yaml      # ClusterIP service
└── app.yaml              # ArgoCD Application manifest
```

---

## ✅ What You Can Try

* Modify `replicas` in `deployment.yaml`, push to GitHub, and sync changes via ArgoCD
* Enable auto-sync in the ArgoCD UI
* Rollback to a previous version
* Add another application and manage it through GitOps

---

## 🔁 Cleanup

```bash
kind delete cluster --name argocd-demo
```

---

## 🧠 Learn More

* [ArgoCD Documentation](https://argo-cd.readthedocs.io/)
* [GitOps Principles](https://www.gitops.tech/)
* [Kind Documentation](https://kind.sigs.k8s.io/)

---

Maintained by [martin-lopez-devops](https://github.com/martin-lopez-devops)
