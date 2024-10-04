## Scenario 2: Handling Node Failure in the Cluster

Scenario Description:
One of the nodes in your Kubernetes cluster has failed, causing some of the pods to become unavailable. You need to quickly recover the failed pods and ensure that service disruption is minimized.Action Plan:

1- Identify the Failed Node: Check the status of all nodes to identify the one that has failed.
```bash
kubectl get nodes
```
Look for nodes in `NotReady` status

![image](https://github.com/user-attachments/assets/f500e32c-f604-4483-b4ad-5fccf2cbf784)



2- Drain the Failed Node: Drain the node to evict all pods safely and prevent new pods from being scheduled.
```bash
kubectl drain <node_name> --ignore-daemonsets --delete-local-data
```
3- Mark the Node as Unschedulable: Cordone the node to mark it as unschedulable.
```bash
kubectl cordon <node_name>
```
4- Schedule Pods on Other Nodes: Kubernetes will automatically attempt to reschedule the evicted pods on other available nodes. Verify the status of your pods.
```bash
kubectl get pods -o wide
```
5- Replace or Repair the Node: If the node cannot be recovered, consider adding a new node to the cluster or repairing the failed node and rejoining it to the cluster.

6- Uncordon the Node (if repaired): If the node has been repaired, uncordon it to allow scheduling.

```bash
kubectl uncordon <node_name>
```

7- Review and Update Node Health Checks: Review and improve your node health checks and alerting mechanisms to detect and respond to node failures faster in the future.
