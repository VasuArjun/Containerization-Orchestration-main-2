
# Azure Kubernetes Service (AKS) Installation Guide

This guide provides instructions on how to create an Azure Kubernetes Service (AKS) cluster using the Azure CLI and the Azure Portal.

## Prerequisites

- Azure account with an active subscription (Free tier is sufficient).
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) installed (for CLI method).
- Local machine configured with [kubectl](https://kubernetes.io/docs/tasks/tools/) (for managing the AKS cluster).

## Method 1: Using Azure CLI

### Step 1: Login to Azure

```bash
az login
```

### Step 2: Create a Resource Group

```bash
az group create --name MyResourceGroup --location eastus
```

### Step 3: Create AKS Cluster

```bash
az aks create \
  --resource-group MyResourceGroup \
  --name MyAKSCluster \
  --node-count 1 \
  --enable-addons monitoring \
  --generate-ssh-keys \
  --node-vm-size Standard_B2s
```

### Step 4: Get Credentials for kubectl

```bash
az aks get-credentials --resource-group MyResourceGroup --name MyAKSCluster
```

### Step 5: Verify the Cluster Nodes

```bash
kubectl get nodes
```

## Method 2: Using Azure Portal

### Step 1: Create a Resource Group

- Navigate to **Resource groups** > **Add**.
- Enter a name and select a location (e.g., `eastus`).
- Click **Review + Create** > **Create**.

### Step 2: Create AKS Cluster

- Search for **Kubernetes services** and select **Add**.
- In the Basics tab:
  - Select your subscription and the resource group you created.
  - Enter a name for the cluster.
  - Choose region and other basic settings as desired.
- In the Node Pools tab:
  - Choose the VM size (e.g., Standard_B2s).
  - Set the node count to 1.
- Enable monitoring under the Monitoring tab.
- Review and create the cluster.

### Step 3: Connect to the Cluster

- Once the cluster is deployed, go to the resource and download the kubeconfig file or open the Cloud Shell.
- Use the following command to configure kubectl:

```bash
az aks get-credentials --resource-group MyResourceGroup --name MyAKSCluster
```

### Step 4: Access Kubernetes Dashboard

- Navigate to your AKS cluster resource.
- Under **Monitoring**, click on **Kubernetes dashboard**.

### Step 5: Clean Up Resources

To avoid incurring unnecessary costs, consider deleting resources if they are no longer needed:

```bash
az aks delete --resource-group MyResourceGroup --name MyAKSCluster --yes --no-wait
az group delete --name MyResourceGroup --yes --no-wait
```

## Conclusion

Following these steps, you will have a functional AKS cluster ready for deploying applications. Be sure to monitor resource usage to stay within the free tier limits.
