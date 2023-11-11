

 As a hosted Kubernetes service, Azure handles critical tasks, like health monitoring and maintenance. 
 When you create an AKS cluster, a control plane is automatically created and configured. This control plane is provided at no cost as a managed Azure resource abstracted from the user.
 AKS nodes run on Azure virtual machines (VMs). With AKS nodes, you can connect storage to nodes and pods, upgrade cluster components, and use GPUs.
 When you deploy an AKS cluster, you specify the number and size of the nodes, and AKS deploys and configures the Kubernetes control plane and nodes.

 
### Documentation

https://learn.microsoft.com/en-us/azure/well-architected/services/compute/azure-kubernetes-service/azure-kubernetes-service


# Network topology

Azure Kubernetes Service cluster is the hub-spoke network topology.
- The hub and spoke(s) are deployed in separate virtual networks connected through peering. Some advantages of this topology are:
  - Segregated management. Enables a way to apply governance and adhere to the principle of least privilege. It also supports the concept of an Azure landing zone with separation of duties.
  - Minimizes direct exposure of Azure resources to the public internet.
  - Organizations often operate with regional hub-spoke topologies. Hub-spoke network topologies can be expanded in the future and provide workload isolation.
  - All web applications should require a web application firewall (WAF) service to help govern HTTP traffic flow.
  - A natural choice for workloads that span multiple subscriptions.
  - It makes the architecture extensible. To accommodate new features or workloads, new spokes can be added instead of redesigning the network topology.
  - Certain resources, such as a firewall and DNS can be shared across networks.

#### The HUB

The hub virtual network is the central point of connectivity and observability. 
The hub contains:
- An Azure Firewall with global firewall policies defined by your central IT teams to enforce organization wide firewall policy
- Azure Bastion
- Azure Monitor for network observability.

Within the network, three subnets are deployed
- Subnet to host Azure Firewall
- Subnet to host a gateway - This subnet is a placeholder for a VPN or ExpressRoute gateway. The gateway provides connectivity between the routers in your on-premises network and the virtual network.
- Subnet to host Azure Bastion - Use Bastion to securely access Azure resources without exposing the resources to the internet. This subnet is used for management and operations only.


#### The SPOKE

The spoke virtual network contains the AKS cluster and other related resources. The spoke has four subnets:
- Subnet to host Azure Application Gateway - Azure Application Gateway is a web traffic load balancer operating at Layer 7. The reference implementation uses the Application Gateway v2 SKU that enables Web Application Firewall (WAF). WAF secures incoming traffic
- Subnet to host the ingress resources - To route and distribute traffic, an ingress controller fulfills the Kubernetes ingress resources. The Azure internal load balancers exist in this subnet.
- Subnet to host the cluster nodes - AKS maintains two separate groups of nodes (or node pools). The system node pool hosts pods that run core cluster services. The user node pool runs your workload and the ingress controller to enable inbound communication to the workload.
- Subnet to host Private Link endpoints - Azure Private Link connections are created for the Azure Container Registry and Azure Key Vault, so these services can be accessed using private endpoint within the spoke virtual network. Private endpoints don't require a dedicated subnet and can also be placed in the hub virtual network. In the baseline implementation, they're deployed to a dedicated subnet within the spoke virtual network. This approach reduces traffic passing the peered network connection and keeps the resources that belong to the cluster in the same virtual network.


# Plan the IP addresses

The address space of the virtual network should be large enough to hold all subnets. 
The address space should be large enough to hold all subnets and allocate IP addresses for all entities that will receive traffic, including nodes, pods, and Azure Private Link addresses.


#### Upgrade

AKS updates nodes regularly to make sure the underlying virtual machines are up to date on security features and other system patches. During an upgrade process, 
AKS creates a node that temporarily hosts the pods, while the upgrade node is cordoned and drained. That temporary node is assigned an IP address from the cluster subnet.

For rolling updates, you need to addresses for the temporary pods that run the workload while the actual pods are updated. If you use the replace strategy, pods are removed, and the new ones are created. So, addresses associated with the old pods are reused.


#### Scalability

Take into consideration the node count of all system and user nodes and their maximum scalability limit. Suppose you want to scale out by 400%. Use four times the number of addresses for all those scaled-out nodes.

In this architecture, each pod can be contacted directly. So, each pod needs an individual address. Pod scalability impacts the address calculation. That decision depends on your choice about the number of pods you want to grow.

Autoscaling is recommended because some of the manual mechanisms are built into the autoscaler, which can scale both pod replicas and the node count in the cluster.

#### Azure Private Link addresses

What is the purpose of deploying Private Link endpoints in a dedicated subnet within the spoke virtual network?
Deploying Private Link endpoints in a dedicated subnet within the spoke virtual network allows for granular security rules to be applied at the subnet level using network security groups, which can help improve security.

Factor in the addresses that are required for communication with other Azure services over Private Link. In this architecture, we have two addresses assigned for the links to Azure Container Registry and Key Vault.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/149e3045-c991-43d1-aab9-c604184fb475)


# Secure the network flow

Network flow can be categorized as:
- Ingress traffic: From the client, to the workload running in the cluster.
- Egress traffic: From a pod or node, in the cluster to an external service.
- Pod-to-pod traffic: Communication between pods. This traffic includes communication between the ingress controller and the workload. Also, if your workload is composed of multiple applications deployed to the cluster, communication between those applications would fall into this category.
- Management traffic:Traffic that goes between the client and the Kubernetes API server.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7ef04d04-9781-4cb2-91e7-4b200488fd0d)


#### Ingress traffic flow

The architecture only accepts TLS encrypted requests from the client. Server Name Indication (SNI) strict is enabled. End-to-end TLS is set up through Application Gateway by using two different TLS certificates, as shown in this image.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4fca814b-1730-4bf4-8679-db96e0686cf6)

1. The client sends an HTTPS request to the domain name: delta.contoso.com. That name is associated with through a DNS A record to the public IP address of Azure Application Gateway. This traffic is encrypted to make sure that the traffic between the client browser and gateway can't be inspected or changed.
2. Application Gateway has an integrated web application firewall (WAF) and negotiates the TLS handshake for delta.contoso.com, allowing only secure ciphers. Application Gateway is a TLS termination point, as it's required to process WAF inspection rules, and execute routing rules that forward the traffic to the configured backend. The TLS certificate is stored in Azure Key Vault. It's accessed using a user-assigned managed identity integrated with Application Gateway.
3. As traffic moves from Application Gateway to the backend, it's encrypted again with another TLS certificate (wildcard for *.aks-ingress.contoso.com) as it's forwarded to the internal load balancer. This re-encryption makes sure traffic that isn't secure doesn't flow into the cluster subnet.
4. The ingress controller receives the encrypted traffic through the load balancer. The controller is another TLS termination point for *.aks-ingress.contoso.com and forwards the traffic to the workload pods over HTTP. The certificates are stored in Azure Key Vault and mounted into the cluster using the Container Storage Interface (CSI) driver.


# LAB - Create an Azure Kubernetes Service cluster

Search for AKS and use the following options: 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c837be7f-030d-4a8f-886a-1b4e133cbf68)


on the NODE POOLS page, select Enable virtual nodes

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/dedb56a8-2dba-4c2b-a157-f307e24ee288)

On the Networking page, select or create a new virtual network.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/423edcce-42ae-4dde-9f18-5f555bb380df)

In the Integrations page, select the Azure container registry you created <newregistryAKS>

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6953c364-2e0b-43aa-8cc7-ddeac2553e39)

In the Advanced and Tags leave as default. Then Create.

In the AKS cluster resource group, on the overview blade select Connect

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/25f31f55-88dd-4289-9929-a272725f28fe)

From the Cloud Shell tab, select Open Cloud Shell

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a9df1624-0e03-4649-bd3c-1b1131441eb6)

```
az account set --subscription

az aks get-credentials --resource-group RG_AKS
```

Copy the commands to the Cloud Shell terminal to set the cluster subscription and download the cluster credentials.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/31a030f7-870c-43dc-8669-5d53f03536ac)

Lets use *kubectl* command to list all deployments in all of namespaces. 

```
kubectl get deployments --all-namespaces=true
```

