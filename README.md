# 🐳 WordPress on Minikube

This project deploys a **WordPress** blog powered by a **MySQL** backend in a **Minikube-based Kubernetes cluster**. It includes all necessary Kubernetes manifests including deployments, services, persistent volumes, secrets, namespaces, network policies, and ingress.

---

## 📁 Project Structure

| File                            | Description                                      |
| ------------------------------- | ------------------------------------------------ |
| `ingress.yaml`                  | Ingress resource for exposing WordPress          |
| `mysql_deployment.yaml`         | MySQL Deployment definition                      |
| `mysql_network_police.yaml`     | NetworkPolicy restricting MySQL access           |
| `mysql_ns.yaml`                 | Namespace for MySQL resources                    |
| `mysql_pvc.yaml`                | PersistentVolumeClaim for MySQL data             |
| `mysql_secret.yaml`             | Secret storing MySQL credentials                 |
| `mysql_service.yaml`            | ClusterIP Service to expose MySQL internally     |
| `wordpress_deployment.yaml`     | WordPress Deployment definition                  |
| `wordpress_network_police.yaml` | NetworkPolicy restricting WordPress access       |
| `wordpress_ns.yaml`             | Namespace for WordPress resources                |
| `wordpress_pvc.yaml`            | PersistentVolumeClaim for WordPress uploads      |
| `wordpress_secret.yaml`         | Secret storing WordPress DB config               |
| `wordpress_service.yaml`        | ClusterIP Service to expose WordPress internally |

---

## 🚀 Getting Started

### 1. Start Minikube

```bash
minikube start
```

### 2. Enable Ingress

```bash
minikube addons enable ingress
```

---

### 3. Apply Manifests

> Make sure you apply manifests in the correct order:

```bash
# Namespaces
kubectl apply -f mysql_ns.yaml
kubectl apply -f wordpress_ns.yaml

# Secrets
kubectl apply -f mysql_secret.yaml
kubectl apply -f wordpress_secret.yaml

# Persistent Volumes
kubectl apply -f mysql_pvc.yaml
kubectl apply -f wordpress_pvc.yaml

# Deployments and Services
kubectl apply -f mysql_deployment.yaml
kubectl apply -f mysql_service.yaml
kubectl apply -f wordpress_deployment.yaml
kubectl apply -f wordpress_service.yaml

# Network Policies
kubectl apply -f mysql_network_police.yaml
kubectl apply -f wordpress_network_police.yaml

# Ingress
kubectl apply -f ingress.yaml
```

---

## 🌐 Access WordPress

After applying the Ingress, run:

```bash
minikube ip
```

Add the following line to your `/etc/hosts` (Linux/macOS) or `C:\Windows\System32\drivers\etc\hosts` (Windows):

```
<minikube-ip> davo.loc
```

Now access:

```
http://davo.loc/wordpress
```

---

## 🔐 Security

This project includes **Network Policies** to:

- Allow only WordPress pods to access MySQL.
- Allow DNS egress from MySQL and WordPress.
- Block all other unnecessary ingress/egress.

Secrets are stored using Kubernetes `Secret` objects to keep credentials secure.

---

## 🧼 Cleanup

To delete all resources:

```bash
kubectl delete ns wordpress
kubectl delete ns mysql
```

To stop Minikube:

```bash
minikube stop
```

---

## 💠 Requirements

- [Minikube](https://minikube.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

---

## 📌 Notes

- This setup is for **local development/testing** only.
- It is not hardened for production environments.

---
