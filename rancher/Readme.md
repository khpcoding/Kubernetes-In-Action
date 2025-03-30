
# 🚀 Rancher Installation on Kubernetes Cluster

## 📌 Overview
Rancher is an open-source platform for managing Kubernetes clusters. It provides a rich UI, APIs, and CLI for managing multiple Kubernetes clusters and workloads. This guide will walk you through the steps to install Rancher on a Kubernetes cluster.

## ✅ Prerequisites 🛠️
- A running Kubernetes cluster (v1.20+ recommended)
- `kubectl` installed
- Helm 3.x installed
- Access to a storage provider (for persistence)

## ⚙️ Installation 🚀
### 1️⃣ Add Rancher Helm Repository
First, add the Rancher Helm chart repository:

```sh
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
helm repo update
```

### 2️⃣ Install Cert-Manager (Required for TLS)
Rancher requires cert-manager for managing TLS certificates. Install it using Helm:

```sh
kubectl create namespace cert-manager
helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.5.4 --set installCRDs=true
```

### 3️⃣ Create Namespace for Rancher
Create a new namespace in your Kubernetes cluster for Rancher:

```sh
kubectl create namespace cattle-system
```

### 4️⃣ Install Rancher
Use Helm to install Rancher in the `cattle-system` namespace:

```sh
helm install rancher rancher-latest/rancher --namespace cattle-system --set hostname=rancher.mycompany.com --set replicas=3
```

### 5️⃣ Check Installation Status
Check the status of your Rancher deployment:

```sh
kubectl -n cattle-system get pods
```
You should see multiple Rancher pods running. Wait until the pods are in the `Running` state.

### 6️⃣ Expose Rancher (Optional)
You can expose Rancher to the internet using a LoadBalancer or an Ingress. If you are using an Ingress, ensure that you have a valid domain and set up an Ingress controller.

Example with Ingress:

```sh
kubectl apply -f rancher-ingress.yaml
```

## 🔄 Access Rancher UI
After the Rancher server is running, you can access the Rancher UI by navigating to `http://rancher.mycompany.com` (or the URL you have configured).

Log in with the default admin user:
- Username: `admin`
- Password: `admin`

**Important:** You will be prompted to change the password on first login.

## 🔧 Post-installation Configuration
After logging in, you can configure Rancher for your environment:
- Add additional Kubernetes clusters
- Set up projects and namespaces
- Configure authentication with external providers (GitHub, LDAP, etc.)

## 🛠️ Troubleshooting 🔍
### Check Rancher Logs
If you encounter any issues, you can check the Rancher logs for more details:

```sh
kubectl -n cattle-system logs -l app=rancher
```

### Check Pods and Services
To ensure all Rancher pods are running correctly:

```sh
kubectl -n cattle-system get pods
kubectl -n cattle-system get svc
```

## 🎯 Conclusion ✅
Rancher is now installed on your Kubernetes cluster. You can use the Rancher UI to manage your clusters and deploy workloads.
