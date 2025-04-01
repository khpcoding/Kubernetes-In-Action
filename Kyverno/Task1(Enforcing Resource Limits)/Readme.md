## 🎯 Scenario Enforcing Resource Limits on Containers**
In this scenario, we'll create a Kyverno policy that ensures all containers have resource limits defined, promoting fair resource allocation and preventing any single container from monopolizing cluster resources

### 1️⃣ Create the Kyverno Policy
Apply the following policy to enforce resource limits on all containers

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: enforce-resource-limits
spec:
  validationFailureAction: Enforce
  rules:
    - name: require-resource-limits
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: "Resource limits are required for all containers."
        pattern:
          spec:
            containers:
              - resources:
                  limits:
                    memory: "?*"
                    cpu: "?*"
```
This policy ensures that every container within a Pod has both memory and CPU limits specified

### 2️⃣ Test the Policy

#### ❌ Attempt to Deploy a Pod Without Resource Limits
Create a Pod definition without resource limits

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-no-limits
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx
```
Attempt to apply this Pod

```sh
kubectl apply -f nginx-no-limits.yaml
```
This deployment will be blocked by Kyverno with a message indicating that resource limits are required

#### ✅ Deploy a Pod With Resource Limits
Modify the Pod definition to include resource limits

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-with-limits
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx
      resources:
        limits:
          memory: "256Mi"
          cpu: "500m"
```
Apply the updated Pod

```sh
kubectl apply -f nginx-with-limits.yaml
```
This deployment will succeed, as it complies with the policy requiring resource limits

---
By implementing this policy, you ensure that all containers running in your Kubernetes cluster have defined resource limits, contributing to the overall stability and efficiency of the cluster

--- **
