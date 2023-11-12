

The scaling applications in Azure Kubernetes Service (AKS), including manually scaling pods or nodes and integrating with Azure Container Instances (ACI).
You need to understand how to scale the applications to meet demand and optimize resource usage.

 
 ### Manually scale pods or nodes

 You can manually scale replicas, or pods, and nodes to test how your application responds to a change in available resources and state. 
 Manually scaling resources lets you define a set amount of resources to use to maintain a fixed cost, such as the number of nodes. 


### Cluster autoscaler

To respond to changing pod demands, the Kubernetes cluster autoscaler adjusts the number of nodes based on the requested compute resources in the node pool. 
By default, the cluster autoscaler checks the Metrics API server every 10 seconds for any required changes in node count.



# LAB - Scale the node count in an Azure Kubernetes Service cluster

If the resource needs of your applications change, your cluster performance may be impacted due to low capacity on CPU, memory, PID space, or disk sizes. To address these changes, you can manually scale your AKS cluster to run a different number of nodes. 
When you scale down, nodes are carefully cordoned and drained to minimize disruption to running applications. When you scale up, AKS waits until nodes are marked Ready by the Kubernetes cluster before pods are scheduled on them.

In the K8s service > Under Settings select Node pools

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/edcbc8e6-9ec5-437c-97b3-a2c9e5961b3c)

Then select the node pool > Scale node pool option

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2af6e01f-458c-47c6-b7f7-92a5d668db6e)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9b7dcba8-a03a-4d2b-bc9e-bdd3bfd2b37a)

Select Autoscale - Recommended and set the minimum and maximum node count. Then, apply.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/781d4c6e-5dcb-4aa7-9442-27e0ad328d75)


### Automatically scale a cluster on Azure Kubernetes Service

Create a resource group using the az group create command.
```
az group create --name myResourceGroup --location eastus
```

Create an AKS cluster using the az aks create command and enable and configure the cluster autoscaler on the node pool for the cluster using the enable-cluster-autoscaler parameter and specifying a node min-count and max-count. 
The following example command creates a cluster with a single node backed by a virtual machine scale set, enables the cluster autoscaler, sets a minimum of one and maximum of three nodes:
```
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --vm-set-type VirtualMachineScaleSets --load-balancer-sku standard --enable-cluster-autoscaler --min-count 1 --max-count 3
```

### Enable the cluster autoscaler on an existing cluster

Update an existing cluster using the az aks update command and enable and configure the cluster autoscaler on the node pool using the enable-cluster-autoscaler parameter and specifying a node min-count andmax-count. 
```
az aks update --resource-group myResourceGroup --name myAKSCluster --enable-cluster-autoscaler --min-count 1 --max-count 3
```

### Disable the cluster autoscaler on a cluster

Disable the cluster autoscaler using the az aks update command and the disable-cluster-autoscaler parameter, Nodes aren't removed when the cluster autoscaler is disabled. 
You can manually scale your cluster after disabling the cluster autoscaler using the az aks scale command.
```
az aks update --resource-group myResourceGroup --name myAKSCluster --disable-cluster-autoscaler
```





























