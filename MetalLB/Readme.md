# 🚀 Deploying Nginx with MetalLB in Kubernetes

## 📌 Overview
[MetalLB](https://metallb.universe.tf/) is a load-balancer implementation for bare metal Kubernetes clusters. This guide provides a simple walkthrough to deploy an Nginx service with MetalLB as the LoadBalancer.

---

## ✅ **Prerequisites**
- A Kubernetes cluster running (v1.20+ recommended).
- `kubectl` and `helm` installed.
- A range of **external IP addresses** available for MetalLB to use.

---

## ⚙️ **Installing MetalLB**
### 1️⃣ Deploy MetalLB Using Helm
```sh
helm repo add metallb https://metallb.github.io/metallb
helm repo update
kubectl create namespace metallb-system
helm install metallb metallb/metallb --namespace metallb-system
```

### 2️⃣ Configure MetalLB IP Address Pool
Create a ConfigMap to define an IP pool range MetalLB will use:

```sh
cat <<EOF | kubectl apply -f -
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: metallb-pool
  namespace: metallb-system
spec:
  addresses:
  - 192.168.1.100-192.168.1.110 # Change this based on your network setup
EOF
```

### 3️⃣ Enable Layer 2 Mode
Create a Layer2 Advertisement to assign external IPs automatically:

```sh
cat <<EOF | kubectl apply -f -
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: l2-advertisement
  namespace: metallb-system
EOF
```

Now, MetalLB is ready to allocate IP addresses!

---

## 🎯 **Deploying Nginx with MetalLB**
### 1️⃣ Deploy Nginx
```sh
kubectl create deployment nginx --image=nginx --port=80
```

### 2️⃣ Expose Nginx Using MetalLB
```sh
kubectl expose deployment nginx --port=80 --type=LoadBalancer
```

### 3️⃣ Verify External IP Allocation
```sh
kubectl get svc nginx
```
_Example Output:_
```
NAME     TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
nginx    LoadBalancer   10.43.0.1      192.168.1.100   80:32423/TCP   2m
```

### 4️⃣ Access Nginx Service
```sh
curl http://192.168.1.100
```
If successful, this will return the default Nginx welcome page.

---

## 🛠️ **Troubleshooting**
### Check MetalLB Controller Logs
```sh
kubectl logs -l app=metallb -n metallb-system
```

### Verify Service External IP Allocation
```sh
kubectl get svc -A
```

---

## 🎯 **Conclusion**
MetalLB is a simple and efficient way to provide Kubernetes services with external IPs in a bare-metal environment. This guide walked you through deploying an Nginx service with MetalLB handling external access. 🎉


