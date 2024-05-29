
# Kubernetes Node Management Commands

This document outlines various `kubectl` commands used for managing nodes in a Kubernetes cluster, providing explanations and examples for each command.

## Listing Nodes
- `kubectl get nodes`: Retrieves a list of all nodes in the cluster.
- `kubectl get nodes -o wide`: Provides a more detailed list of nodes, including additional info such as the IP address, operating system, and kernel version.

## Describing a Node
- `kubectl describe node kube-node`: Shows detailed information about a specific node called `kube-node`, including its conditions, capacities, allocations, and events.

## Filtering Nodes
- `kubectl get node --selector='!node-role.kubernetes.io/master'`: Lists all worker nodes by excluding nodes labeled as master.
- `kubectl get nodes --show-labels`: Displays all nodes along with their labels.
- `kubectl get node --selector='env=prod'`: Lists all nodes that have the label `env=prod`.

## Labeling a Node
- `kubectl label node kube-node env=prod`: Adds the label `env=prod` to the node named `kube-node`.

## Managing Taints
- `kubectl taint nodes node1 key=value:NoSchedule`: Applies a taint to `node1` that prevents any pod from being scheduled on this node unless it has a toleration matching the taint.
- `kubectl taint nodes node1 key:NoSchedule-`: Removes the specified taint from `node1`.
  
- Taints can have three effects:
  
   - NoSchedule: Pods will not be scheduled on the node unless they tolerate the taint.
  
   - PreferNoSchedule: Kubernetes will try to avoid placing a pod on the node but it's not guaranteed.

   - NoExecute: Pods that do not tolerate this taint will be evicted if they are already running on the node, and will not 
     be scheduled on it in the future.
  
- To allow pods to be scheduled on a node with a taint, you need to add a toleration to the pod specification that matches the taint on the node. Below, I'll give you an example of how to set up a taint on a node and then how to configure a pod with a toleration so that it can be scheduled on that tainted node.

Step 1: Taint a Node
First, let’s assume you have a taint on a node. Here's how you might add a taint to a node:

```
kubectl taint nodes node1 key1=value1:NoSchedule
```

This command adds a taint to node1 where the key is key1, the value is value1, and the effect is NoSchedule. This means that no pod will be scheduled on node1 unless it has a matching toleration.

Step 2: Pod Specification with Toleration
Next, here's how you would define a pod specification that includes a toleration matching the taint:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: myimage
  tolerations:
  - key: "key1"
    operator: "Equal"
    value: "value1"
    effect: "NoSchedule"
```

In this YAML:

The pod mypod is configured to run a container using the image myimage.
Under tolerations, the pod includes a toleration that matches the taint on node1. This toleration uses:
key: Must match the key of the taint (key1).
operator: Specifies the operation, which is Equal in this case.
value: Must match the value of the taint (value1).
effect: Specifies the effect to tolerate, which must match the effect specified in the taint (NoSchedule).

## Viewing and Editing Node Configuration
- `kubectl get node kube-node -o yaml`: Displays the configuration of `kube-node` in YAML format.
- `kubectl edit node kube-node`: Opens the configuration of `kube-node` in an editor, allowing you to modify and apply changes directly.

## Managing Node Scheduling
- `kubectl cordon kube-node`: Marks `kube-node` as unschedulable, which prevents new pods from being scheduled on this node while not disturbing existing workloads.
- `kubectl uncordon kube-node`: Marks `kube-node` as schedulable again, allowing pods to be scheduled on it.
- `kubectl drain kube-node`: Prepares `kube-node` for maintenance by safely evicting all pods from the node. Useful before performing tasks like kernel upgrades, hardware maintenance, etc.
  - `kubectl drain kube-node --ignore-daemonsets --force`: Forces draining by ignoring DaemonSet-managed pods and safety checks.

## Deleting a Node
- `kubectl delete node kube-node`: Removes `kube-node` from the cluster.
