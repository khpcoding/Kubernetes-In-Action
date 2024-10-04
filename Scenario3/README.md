## Implementing Blue-Green Deployment

Scenario Description:
You want to deploy a new version of your application without affecting the live production environment. You decide to use a Blue-Green deployment strategy to minimize risk and ensure zero downtime.

Action Plan:

1- Deploy the New Version in Parallel: Create a new deployment (green) with the same configurations as your current deployment (blue), but using the updated version of your application.

```bash
kubectl apply -f deployment-green.yaml
```
2- Set Up a Service: Create a service that points to the blue deployment initially.

```bash
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app-blue
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```
3- Test the Green Deployment: Verify that the green deployment is functioning correctly by testing it through a temporary service or using port forwarding.

4- Switch the Service to Green: Update the service to point to the green deployment.

```bash
selector:
  app: my-app-green
```
5- Monitor the Application: Monitor the new deployment to ensure that everything is working correctly.

6- Delete the Blue Deployment: Once you confirm that the new version is stable, delete the old (blue) deployment.

```bash
kubectl delete deployment my-app-blue
```
7- Update Documentation: Document the deployment and switch details for future reference.

These scenarios cover various situations you might encounter when working with Kubernetes clusters, providing actionable steps to handle them effectively.
