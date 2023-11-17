
https://learn.microsoft.com/en-us/training/modules/protect-virtual-desktop-deployment-azure-firewall/3-exercise-set-up-host-pool


----- Azure Firewall

Azure Firewall is a managed network security service that's cloud-based and that protects your Azure Virtual Network resources. 
It's a fully stateful and centralized network firewall as a service. It provides network-level and application-level protection across different subscriptions and virtual networks.

Azure Firewall is provisioned inside a hub virtual network. Traffic to and from the spoke virtual networks and the on-premises network traverses the firewall within the hub network.

Azure Firewall can work with internal traffic filtering includes spoke-to-spoke traffic and hybrid cloud traffic between your on-premises network and your Azure virtual network.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8059215c-ed1e-4276-8acd-4f1610e27933)


When an end user connects to an Azure Virtual Desktop (Azure Virtual Desktop is a desktop and app virtualization service that runs in the cloud) virtual machine, that virtual machine belongs to a host pool. 

A host pool is a collection of Azure virtual machines (VMs) that register to the Azure Virtual Desktop service as session hosts. These VMs run in an Azure virtual network and are subject to virtual network security controls.

For Azure Virtual Desktop to work, the host pool needs outbound internet access to the Azure Virtual Desktop service. The host pool might also need outbound internet access for your users.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5980ef44-9563-4cd7-9c27-f5b2f14a6038)

