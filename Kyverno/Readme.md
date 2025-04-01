# 🚀 Deploying and Using Kyverno in Kubernetes

## 📌 Overview
[Kyverno](https://kyverno.io/) is a Kubernetes-native policy management tool that helps enforce security, compliance, and governance policies. Unlike other policy engines, Kyverno operates using native Kubernetes resources.

---

## ✅ **Prerequisites**
- A Kubernetes cluster (v1.20+ recommended)
- `kubectl` and `helm` installed
- Administrator access to the cluster

---

## ⚙️ **Installing Kyverno**
### 1️⃣ Deploy Kyverno Using Helm
```sh
helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
helm install kyverno kyverno/kyverno -n kyverno --create-namespace
```

### 2️⃣ Verify Kyverno Installation
```sh
kubectl get pods -n kyverno
```
You should see Kyverno pods running in the `kyverno` namespace.

---

## 🎯 **Scenario: Enforcing Image Registries**
This scenario ensures that only images from an approved registry (e.g., Docker Hub) can be deployed in the cluster.

### 1️⃣ Create a Policy to Restrict Image Registries
```sh
cat <<EOF | kubectl apply -f -
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-image-registries
spec:
  validationFailureAction: Enforce
  rules:
    - name: validate-image-registry
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: "Only images from docker.io are allowed."
        pattern:
          spec:
            containers:
              - image: "docker.io/*"
EOF
```

### 2️⃣ Test the Policy
#### ✅ Allowed Deployment (Docker Hub)
```sh
kubectl run nginx --image=docker.io/nginx
```
#### ❌ Denied Deployment (Other Registry)
```sh
kubectl run nginx --image=quay.io/nginx
```
This deployment will be blocked with an error message from Kyverno.

---

## 🛠️ **Troubleshooting**
### Check Kyverno Logs
```sh
kubectl logs -l app=kyverno -n kyverno
```

### List Applied Policies
```sh
kubectl get cpol -A
```

---

## 🎯 **Conclusion**
Kyverno provides a simple way to enforce security and compliance policies in Kubernetes using native resources. This guide demonstrated how to install Kyverno and enforce an image registry restriction policy. 🚀



