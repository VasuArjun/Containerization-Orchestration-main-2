# Kubernetes Architecture Overview

## Introduction
Kubernetes is a system for managing containerized applications across a cluster of machines. It provides tools for deploying applications, scaling them as necessary, managing changes to existing containerized applications, and helps optimize the use of underlying hardware beneath your containers.



## Master Node Components

- **API Server (kube-apiserver)**:
  - The API server is a component of the Kubernetes control plane that exposes the Kubernetes API.
  - It is the front-end for the Kubernetes control plane.
  - It allows users, management tools, and other Kubernetes components to communicate with the cluster.

- **etcd**:
  - Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.
  - It stores the entire configuration data of the cluster and the state including pods, services, and other resources.

- **Scheduler (kube-scheduler)**:
  - Watches for newly created Pods with no assigned node, and selects a node for them to run on.
  - Factors taken into account for scheduling decisions include individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.

- **Controller Manager (kube-controller-manager)**:
  - Runs controller processes.
  - These controllers include:
    - Node Controller: Responsible for noticing and responding when nodes go down.
    - Replication Controller: Responsible for maintaining the correct number of pods for every replication controller object in the system.

## Worker Node Components

- **Kubelet**:
  - An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.
  - It takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy.

- **Kube-Proxy**:
  - Maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.
  - Kube-Proxy uses the operating system packet filtering layer if there is one and it's available.

- **Container Runtime**:
  - The software that is responsible for running containers.
  - Kubernetes supports several container runtimes: Docker, containerd, cri-o, rktlet, and any implementation of the Kubernetes CRI (Container Runtime Interface).

- **Pods**:
  - The smallest deployable units of computing that can be created and managed in Kubernetes.
  - A Pod is a group of one or more containers, with shared storage/network resources, and a specification for how to run the containers.

## Conclusion
Kubernetes' architecture is designed to facilitate highly available, scalable, and flexible container orchestration. Understanding these components is crucial for effectively deploying, managing, and scaling applications within a Kubernetes environment.
