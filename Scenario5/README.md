# 🧭 Real-World Scenario: Using Node Affinity in Kubernetes

In large-scale Kubernetes clusters, it's common to have nodes dedicated to specific teams, environments (e.g., dev/stage/prod), or workloads (e.g., GPU-heavy, storage-optimized). Node Affinity helps ensure that pods are scheduled only on the appropriate nodes based on **custom labels**.

---

## 📘 Scenario: Deploying NGINX Only on BI Team Nodes

### 🎯 Goal:
You want to ensure that an `nginx` pod is deployed **only on nodes** that belong to the **Business Intelligence (BI) Team**.

Let’s assume you've labeled certain nodes with:

```bash
kubectl label nodes worker-node-3 nodename=bi-team
```
---

## ⚙️ How It Works

This example shows how to force a pod to be scheduled **only** on nodes labeled as part of the **BI (Business Intelligence) team**.

- The pod will only be scheduled on nodes with the label:
  ```bash
  nodename=bi-team
  ```
  ## ✅ Steps to Try It Yourself
   1- Label a node for the BI team (if not already labeled):
   ```bash
   kubectl label node worker-node-3 nodename=bi-team
   ```
   2- Apply the Node Affinity pod manifest\
   ```bash
  kubectl apply -f nginx-node-affinity.yaml
  ```
   3- Check the pod status and assigned node:
   ```bash
   kubectl get pods -o wide
   ```
## 📝 Best Practices

    Use meaningful labels for your nodes, such as:
    
    `team=bi`
    
    `env=prod`
    
    `zone=us-east1-b`
    
    Use the following types of affinity based on scheduling needs:
    
    `requiredDuringSchedulingIgnoredDuringExecution`: for strict rules.
    
    `preferredDuringSchedulingIgnoredDuringExecution`: for soft preferences.
  
  Combine node affinity with tolerations if you are also using taints on your nodes to enforce advanced scheduling behavior.
  
