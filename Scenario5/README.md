# 🧭 Real-World Scenario: Using Node Affinity in Kubernetes

In large-scale Kubernetes clusters, it's common to have nodes dedicated to specific teams, environments (e.g., dev/stage/prod), or workloads (e.g., GPU-heavy, storage-optimized). Node Affinity helps ensure that pods are scheduled only on the appropriate nodes based on **custom labels**.

---

## 📘 Scenario: Deploying NGINX Only on BI Team Nodes

### 🎯 Goal:
You want to ensure that an `nginx` pod is deployed **only on nodes** that belong to the **Business Intelligence (BI) Team**.

Let’s assume you've labeled certain nodes with:

```bash
kubectl label nodes worker-node-3 nodename=bi-team

---

## ⚙️ How It Works

This example shows how to force a pod to be scheduled **only** on nodes labeled as part of the **BI (Business Intelligence) team**.

- The pod will only be scheduled on nodes with the label:
  ```bash
  nodename=bi-team
