# 🚀 MetalLB: Load Balancer for Kubernetes

## 📌 Overview
[MetalLB](https://metallb.universe.tf/) is a load-balancer implementation for bare metal Kubernetes clusters. It allows services to have externally accessible IPs, similar to cloud provider LoadBalancer services.

### ✅ **Why Use MetalLB?**
- Provides **LoadBalancer-type services** in on-premise or bare metal Kubernetes clusters.
- Supports **Layer 2 (ARP/NDP) and BGP (Border Gateway Protocol) modes**.
- Lightweight and easy to deploy.
- Works seamlessly with Kubernetes networking.

---

## ✅ **Prerequisites**
- A Kubernetes cluster running (v1.20+ recommended).
- `kubectl` and `helm` installed.
- A range of **external IP addresses** available for MetalLB to use.
- A working network setup that allows external access.

---

## ⚙️ **Installation of MetalLB**
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

## 🎯 **Scenarios & Use Cases**

### 🔹 **Scenario 1: Exposing a Simple Nginx Service**
Deploy a simple Nginx web server and expose it using a **LoadBalancer** service:

#### **Step 1: Deploy Nginx**
```sh
kubectl create deployment nginx --image=nginx --port=80
kubectl expose deployment nginx --port=80 --type=LoadBalancer
```

#### **Step 2: Verify External IP Allocation**
```sh
kubectl get svc nginx
```
_Example Output:_
```
NAME     TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
nginx    LoadBalancer   10.43.0.1      192.168.1.100   80:32423/TCP   2m
```
Try accessing the service:
```sh
curl http://192.168.1.100
```

### 🔹 **Scenario 2: Using MetalLB with BGP for Dynamic Routing**
BGP mode allows MetalLB to peer with routers and advertise service IPs dynamically.

#### **Step 1: Define a BGP Configuration**
```sh
cat <<EOF | kubectl apply -f -
apiVersion: metallb.io/v1beta1
kind: BGPPeer
metadata:
  name: bgp-peer
  namespace: metallb-system
spec:
  peerAddress: 192.168.1.1 # Replace with your router IP
  peerASN: 65001
  myASN: 65002
EOF
```

#### **Step 2: Deploy an Application and Verify BGP Routing**
```sh
kubectl create deployment webapp --image=nginx --port=80
kubectl expose deployment webapp --port=80 --type=LoadBalancer
kubectl get svc webapp
```
If configured correctly, the router will distribute the IP dynamically.

### 🔹 **Scenario 3: High Availability with Multiple Nodes**
To prevent a single point of failure, ensure multiple worker nodes can serve external traffic.
- Use **Layer 2 mode** for local network load balancing.
- Use **BGP mode** for efficient routing over enterprise networks.
- Deploy a **replicated service** (e.g., `nginx`) with multiple pods across nodes.

---

## 🛠️ **Troubleshooting & Debugging**
### Check MetalLB Controller Logs
```sh
kubectl logs -l app=metallb -n metallb-system
```

### Verify Service External IP Allocation
```sh
kubectl get svc -A
```

### Debug BGP Session
```sh
kubectl get bgppeers -n metallb-system
```

---

## 🎯 **Conclusion**
MetalLB is a robust solution for providing Kubernetes services with external IPs in a bare-metal environment. Whether using Layer 2 mode for small-scale clusters or BGP for dynamic routing, MetalLB simplifies network load balancing for Kubernetes workloads.


