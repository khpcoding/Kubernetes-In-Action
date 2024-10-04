## Scenario 1: Application Deployment and Scaling

Scenario Description:
Your team has developed a new microservice-based application and wants to deploy it on a Kubernetes cluster. The application consists of multiple microservices, each with different resource requirements. You need to deploy, configure, and scale these services to ensure they can handle production traffic.

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: my-app-image:v1.0
        ports:
        - containerPort: 8080
```

Action Plan:
1- Create Deployment Manifests: Create a deployment.yaml file for each microservice with appropriate specifications (e.g., image, resource limits, replicas).
2- Deploy the Services: Apply the manifest files to the cluster.
```bash
kubectl apply -f deployment.yaml
```
3- Configure Horizontal Pod Autoscaler (HPA): Set up an HPA for each deployment to scale based on CPU utilization.
```bash
kubectl autoscale deployment my-app --cpu-percent=80 --min=3 --max=10
```
4- Expose the Services: Create a Service object to expose the microservices for internal or external access.
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```
5- Monitor and Adjust: Use monitoring tools like Prometheus and Grafana to monitor CPU and memory utilization and make adjustments as necessary.
