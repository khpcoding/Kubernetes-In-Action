## Scenario 4 : Rolling Update of an Application

Scenario Description:
You want to deploy a new version of your application without downtime. Using Kubernetes rolling update strategy, you want to ensure that users experience a smooth transition with zero downtime.

Action Plan:

1- Create a Deployment with the New Version: Edit your deployment to update the container image to the new version.

```bash
spec:
  containers:
  - name: my-app-container
    image: my-app-image:v2.0
```
2- Apply the Update: Apply the updated deployment file.
```bash
kubectl apply -f deployment.yaml
```
3- Monitor the Rolling Update: Check the status of the rolling update to ensure that it proceeds as expected.
```bash
kubectl rollout status deployment/my-app
```
4- Verify Application Health: Test the new version of the application to ensure it is functioning correctly.
5- Roll Back if Necessary: If the new version causes issues, you can roll back to the previous version.
```bash
kubectl rollout undo deployment/my-app
```
6- Update Runbooks and Documentation: Document the new deployment, including any specific configurations or performance metrics observed.
