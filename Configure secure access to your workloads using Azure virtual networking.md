


============ configure Virtual Networks

Suppose your company is migrating to Azure. They want to replicate their on-premises network in the cloud. Their Azure resources need to be organized into virtual networks and subnets, and implement a customized IP addressing schema for the company.

You're responsible for configuring the virtual networks and subnets, including IP addressing. The company schema should provide flexibility, room for growth, and integration with on-premises networks. 
The schema should also minimize public exposure of systems, and give the organization flexibility in its network design.
If your network configuration isn't properly designed, systems might not be able to communicate.


Describe Azure Virtual Network features and components.
Identify features and usage cases for subnets and subnetting.
Identify usage cases for private and public IP addresses.
Create and determine which resources require public IP addresses.
Create and determine which resources require private IP addresses.
Create virtual networks.


Plan virtual networks
Moving resources to the cloud can save money and simplify administrative operations. Relocating resources removes the need to maintain expensive datacenters with uninterruptible power supplies, generators, multiple fail-safes, or clustered database servers
This is good for small and medium size companies

After resources are moved to Azure, they require the same networking functionality as an on-premises deployment. In specific scenarios, the resources require some level of network isolation. Azure network services offer a range of components with functionalities and capabilities, 
like
virtual network - Azure virtual network is a logical isolation of the Azure cloud that's dedicated to your subscription.
load balancer
application gateway
traffic manager profile
virtual network gateway
virtual WAN


You can use virtual networks to provision and manage virtual private networks (VPNs) in Azure.
Each virtual network has its own Classless Inter-Domain Routing (CIDR) block and can be linked to other virtual networks and on-premises networks.
You can link virtual networks with an on-premises IT infrastructure to create hybrid or cross-premises solutions, when the CIDR blocks of the connecting networks don't overlap.
You control the DNS server settings for virtual networks, and segmentation of the virtual network into subnets.
You can't redefine the IP address space for a network after it's created. If you plan your address space for cloud-only virtual networks, you might later decide to connect an on-premises site.

Things to consider when using virtual networks
. When you create a virtual network, your services and virtual machines within your virtual network can communicate directly and securely with each other in the cloud. You can still configure endpoint connections for the virtual machines and services that require internet communication, as part of your solution.
You can build traditional site-to-site VPNs to securely scale your datacenter capacity. Site-to-site VPNs use IPSEC to provide a secure connection between your corporate VPN gateway and Azure.
Virtual networks give you the flexibility to support a range of hybrid cloud scenarios. You can securely connect cloud-based applications to any type of on-premises system, such as mainframes and Unix systems.

Azure Virtual Network connects Azure resources including virtual machines, the Azure App Service Environment, Azure Kubernetes Service (AKS), and Azure Virtual Machine Scale Sets. You can use service endpoints to connect to other Azure resource types, such as Azure SQL databases and storage accounts.

 The resources can communicate with each other, the internet, and on-premises networks. Azure Virtual Network is similar to a traditional network, but offers more benefits such as scale, availability, and isolation.

 

Subnets provide a way for you to implement logical divisions within your virtual network. Your network can be segmented into subnets to help improve security, increase performance, and make it easier to manage.

Each subnet contains a range of IP addresses that fall within the virtual network address space.
The address range for a subnet must be unique within the address space for the virtual network.
The range for one subnet can't overlap with other subnet IP address ranges in the same virtual network.
The IP address space for a subnet must be specified by using CIDR notation.
You can segment a virtual network into one or more subnets in the Azure portal. Characteristics about the IP addresses for the subnets are listed.

For each subnet, Azure reserves five IP addresses. The first four addresses and the last address are reserved.

Reserved address	Reason
192.168.1.0	This value identifies the virtual network address.
192.168.1.1	Azure configures this address as the default gateway.
192.168.1.2 and 192.168.1.3	Azure maps these Azure DNS IP addresses to the virtual network space.
192.168.1.255	This value supplies the virtual network broadcast address.


Things to consider when using subnets  - https://learn.microsoft.com/en-us/azure/networking/
Consider service requirements. Each service directly deployed into a virtual network has specific requirements for routing and the types of traffic that must be allowed into and out of associated subnets. A service might require or create their own subnet. There must be enough unallocated space to meet the service requirements. Suppose you connect a virtual network to an on-premises network by using Azure VPN Gateway. The virtual network must have a dedicated subnet for the gateway.
Consider network virtual appliances. Azure routes network traffic between all subnets in a virtual network, by default. You can override Azure's default routing to prevent Azure routing between subnets. You can also override the default to route traffic between subnets through a network virtual appliance. If you require traffic between resources in the same virtual network to flow through a network virtual appliance, deploy the resources to different subnets.
Consider service endpoints. You can limit access to Azure resources like an Azure storage account or Azure SQL database to specific subnets with a virtual network service endpoint. You can also deny access to the resources from the internet. You might create multiple subnets, and then enable a service endpoint for some subnets, but not others.
Consider network security groups. You can associate zero or one network security group to each subnet in a virtual network. You can associate the same or a different network security group to each subnet. Each network security group contains rules that allow or deny traffic to and from sources and destinations.
Consider private links. Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. Private Link simplifies the network architecture and secures the connection between endpoints in Azure. The service eliminates data exposure to the public internet.


Plan IP addressing    -   https://learn.microsoft.com/en-us/training/modules/design-ip-addressing-for-azure/

Private IP addresses enable communication within an Azure virtual network and your on-premises network. You create a private IP address for your resource when you use a VPN gateway or Azure ExpressRoute circuit to extend your network to Azure.

A private IP address resource can be associated with virtual machine network interfaces, internal load balancers, and application gateways. 

Dynamic: Azure assigns the next available unassigned or unreserved IP address in the subnet's address range. Dynamic assignment is the default allocation method.

Suppose addresses 10.0.0.4 through 10.0.0.9 are already assigned to other resources. In this case, Azure assigns the address 10.0.0.10 to a new resource.

Static: You select and assign any unassigned or unreserved IP address in the subnet's address range.

Suppose a subnet's address range is 10.0.0.0/16, and addresses 10.0.0.4 through 10.0.0.9 are already assigned to other resources. In this scenario, you can assign any address between 10.0.0.10 and 10.0.255.254.

Static IP addresses don't change and are best for certain situations, such as:

DNS name resolution, where a change in the IP address requires updating host records.
IP address-based security models that require apps or services to have a static IP address.
TLS/SSL certificates linked to an IP address.
Firewall rules that allow or deny traffic by using IP address ranges. 
Role-based virtual machines such as Domain Controllers and DNS servers.



Public IP addresses allow your resource to communicate with the internet. You can create a public IP address to connect with Azure public-facing services.
You can create a public IP address for your resource in the Azure portal.

IP Version: Select to create an IPv4 or IPv6 address, or Both addresses. The Both option creates two public IP addresses: an IPv4 address and an IPv6 address.
SKU: Select the SKU for the public IP address, including Basic or Standard. The value must match the SKU of the Azure load balancer with which the address is used.
IP address assignment: Identify the type of IP address assignment to use.

Dynamic addresses are assigned after a public IP address is associated to an Azure resource and is started for the first time. Dynamic addresses can change if a resource such as a virtual machine is stopped (deallocated) and then restarted through Azure. The address remains the same if a virtual machine is rebooted or stopped from within the guest OS. When a public IP address resource is removed from a resource, the dynamic address is released.
(If the VM is in de-allocated state, then its IP gets detached (if a static IP is not assigned). Stopped: This happens if you shut down the instance through the machine itself.)
Azure’s Stopped State
When you are logged in to the operating system of an Azure VM, you can issue a command to shut down the server. This will kick you out of the OS and stop all processes, but will maintain the allocated hardware (including the IP addresses currently assigned). If you find the VM in the Azure console, you’ll see the state listed as “Stopped”. The biggest thing you need to know about this state is that you are still being charged by the hour for this instance.
Azure’s Deallocated State
The other way to stop your virtual machine is through Azure itself, whether that’s through the console, Powershell, or the Azure CLI. When you stop a VM through Azure, rather than through the OS, it goes into a “Stopped (deallocated)” state. This means that any non-static public IPs will be released, but you’ll also stop paying for the VM’s compute costs.

Static addresses are assigned when a public IP address is created. Static addresses aren't released until a public IP address resource is deleted. If the address isn't associated to a resource, you can change the assignment method after the address is created. If the address is associated to a resource, you might not be able to change the assignment method.
If you select IPv6 for the IP version, the assignment method must be Dynamic for the Basic SKU. Standard SKU addresses are Static for both IPv4 and IPv6 addresses.


---lab

Your organization is migrating network infrastructure and virtual machines to Azure. 
What you need to do:
Configure Azure virtual networks and subnets.
Connect remotely to Azure virtual machines by using RDP.
Verify virtual machines in the same virtual network can communicate.

Task 1: Create a virtual network.   https://learn.microsoft.com/en-us/training/modules/design-ip-addressing-for-azure/
Create a virtual network, vnet1, with an IP address space of 10.1.0.0/16.
Create a subnet, default, with an IP address space of 10.1.0.0/24.
Task 2: Create two virtual machines.
Create a virtual machine, vm1, in vnet1 and allow inbound RDP.
Create a second virtual machine, vm2, in vnet1 and allow inbound RDP.
Ensure both virtual machines are deployed and running before continuing.
Task 3: Test the virtual machine connections.
Connect to vm1 with RDP.
Connect to vm2 with RDP.
Disable the public and private Windows Defender firewall on both virtual machines.
Use Azure PowerShell to confirm vm1 can ping vm2.

create a resource group (click on new and type a name as you wish)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c8c32343-b10f-4efb-8b43-ff5abcea038f)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fcd11c8d-0090-4db7-90c6-52ad888b84f0)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fc500b20-51ff-4b70-96d1-1914ded15132)

colocar o IP desejado

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/cc2a5627-bfc7-4b25-b83f-26c12f720e29)

ADD subnet

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fa126e43-3e1a-4112-b2d0-9a52d494f5ca)


Then create and review, then create

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2dc9c4c3-2386-42d1-8cfd-fe9311d682b4)


lets create the VMs, the same page above, click on GO to Resource

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a9e7a6d4-7af0-4c40-a33e-f5625b930ba3)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7bc220b3-2410-4784-a7d0-b2de3f078f2c)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a1ee5024-6df0-481b-8064-25433b7f1499)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/aba997c8-874a-47e8-810e-c62dcfa33214)


Test the connectivity

select RDP and use the default port 3389, download the RDP file

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fbb41edd-8301-4c45-9a55-ee2ae293b132)

open the download file and enter the credentials, 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/33a984b5-299e-4b02-b4ec-beb2e4d0817b)

before ping, need to disable the firewall in both vms. Then on powershell ping the hostnmae vm2

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1d1276dc-227c-4449-b942-a440db2536d9)





=============== Configure Azure Virtual Network peering  - https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/

Azure Virtual Network peering lets you connect virtual networks in the same or different regions, so resources in both networks can communicate with each other.
Azure Virtual Network peering lets you connect virtual networks in a hub and spoke topology. Virtual Network peering is a cost-effective and easy to configure solution for connecting your Azure virtual networks.


Identify usage cases and product features of Azure Virtual Network peering.
Configure your network to implement Azure VPN Gateway for transit connectivity.
Extend peering by using a hub and spoke network with user-defined routes and service chaining.

Perhaps the simplest and quickest way to connect your virtual networks is to use Azure Virtual Network peering. Virtual Network peering enables you to seamlessly connect two Azure virtual networks. After the networks are peered, the two virtual networks operate as a single network, for connectivity purposes.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/da37ba07-0011-44a0-9e1f-dee0fa7205ca)

After you create a peering between virtual networks, the individual virtual networks are still managed as separate resources.

When you implement Azure Virtual Network peering, network traffic between peered virtual networks is private. Traffic between the virtual networks is kept on the Microsoft Azure backbone network. No public internet, gateways, or encryption is required in the communication between the virtual networks.
 utilizes the Azure infrastructure, you gain a low-latency, high-bandwidth connection between resources in different virtual networks.
You can create an Azure Virtual Network peering configuration to transfer data across Azure subscriptions, deployment models, and across Azure regions.

--- When virtual networks are peered, you can configure Azure VPN Gateway in the peered virtual network as a transit point.

Consider a scenario where three virtual networks in the same region are connected by virtual network peering. Virtual network A and virtual network B are each peered with a hub virtual network. The hub virtual network contains several resources, including a gateway subnet and an Azure VPN gateway. The VPN gateway is configured to allow VPN gateway transit. Virtual network B accesses resources in the hub, including the gateway subnet, by using a remote VPN gateway.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/bb0cb775-91aa-4434-86fb-d326d7e463ba)

A virtual network can have only one VPN gateway.
Gateway transit is supported for both regional and global virtual network peering.

When you allow VPN gateway transit, the virtual network can communicate to resources outside the peering. In our sample illustration, the gateway subnet gateway within the hub virtual network can complete tasks such as:
Use a site-to-site VPN to connect to an on-premises network.
Use a vnet-to-vnet connection to another virtual network.
Use a point-to-site VPN to connect to a client.

Gateway transit allows peered virtual networks to share the gateway and get access to resources. With this implementation, you don't need to deploy a VPN gateway in the peer virtual network.

You can apply network security groups in a virtual network to block or allow access to other virtual networks or subnets. When you configure virtual network peering, you can choose to open or close the network security group rules between the virtual networks.

To implement virtual network peering, your Azure account must be assigned to the Network Contributor or Classic Network Contributor role. Alternatively, your Azure account can be assigned to a custom role that can complete the necessary peering actions. 

To create a peering, you need two virtual networks. The second virtual network in the peering is referred to as the remote network.Initially, the virtual machines in your virtual networks can't communicate with each other. After the peering is established, the machines can communicate within the peered network based on your configuration settings. 



Choose the first virtual network to use in the peering, and select Settings > Add (peering).

Configure the peering parameters for the first virtual network.
The top portion of the Add peering dialog shows settings for this virtual network. The bottom portion of the dialog shows settings for the remote virtual network in the peering.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6c44153c-381c-46aa-a6aa-4a5265d9c140)

Peering link name: Provide a name to identify the peering on this virtual network. The name must be unique within the virtual network.

Traffic to remote virtual network: Specify how to control traffic to the remote virtual network.

Allow: Allow communication between resources connected to both of your virtual networks within the peered network.

Block: Block all traffic to the remote virtual network. You can still allow some traffic to the remote virtual network if you explicitly open the traffic through a network security group rule.

Traffic forwarded from remote virtual network: Specify how to control traffic that originates from outside your remote virtual network.

Allow: Forward outside traffic in the remote virtual network to this virtual network within the peering. This parameter lets you forward traffic from outside the remote virtual network, such as traffic from an NVA, to this virtual network.

Block: Block the forwarding of outside traffic from the remote virtual network to this virtual network within the peering. Again, some traffic can still be forwarded by explicitly opening the traffic through a network security group rule. When you configure traffic forwarding between virtual networks through an Azure VPN gateway, this parameter isn't applicable.

Virtual network gateway or Route Server: Specify whether your virtual network peering should use an Azure VPN gateway. The default is to not use a VPN gateway (None).


Configure the peering parameters for your remote virtual network.

In the Azure portal, you configure the remote virtual network in the peering on the Add peering dialog. The bottom portion shows settings for the remote virtual network. The settings are similar to the parameters described for the first virtual network.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/485a85f4-48ef-48e3-ac69-1c004ab52028)

Create at least one virtual machine in each virtual network.

Test communication between the virtual machines within your peered network.

Your peering isn't successfully established until both virtual networks in the peering have a status of Connected.


--- Extend peering with user-defined routes and service chaining

Suppose you have three virtual networks: A, B, and C. You establish virtual network peering between networks A and B, and also between networks B and C. You don't set up peering between networks A and C. The virtual network peering capabilities that you set up between networks B and C don't automatically enable peering communication capabilities between networks A and C.

There are a few ways to extend the capabilities of your peering for resources and virtual networks outside your peering network:

Hub and spoke networks
User-defined routes
Service chaining

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/846f72fc-6684-4181-8631-2f5a367e836f)

Hub and spoke network
When you deploy a hub-and-spoke network, the hub virtual network can host infrastructure components like a network virtual appliance (NVA) or Azure VPN gateway. All the spoke virtual networks can then peer with the hub virtual network. Traffic can flow through NVAs or VPN gateways in the hub virtual network.

User-defined route (UDR)
Virtual network peering enables the next hop in a user-defined route to be the IP address of a virtual machine in the peered virtual network, or a VPN gateway.

Service chaining	
Service chaining lets you define UDRs. These routes direct traffic from one virtual network to an NVA or VPN gateway.

-------- lab    implement inter-site connectivity

Your organization has three datacenters connected with a mesh wide-area network. As the Azure Administrator, you need to implement the on-premises infrastructure in Azure.

There are two offices, New York and Boston, in one region.
There's one office, Seattle, in another region.
All the offices need to be networked together so they can share information.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/da30b1cd-8940-40fd-a4ea-b318d06e7a9c)

Task 1: Create the infrastructure environment. In this task, you'll deploy three virtual machines. Virtual machines will be deployed in different regions and virtual networks.
Use a template to create the virtual networks and virtual machines in the different regions. You can review the lab template. (https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/blob/master/Allfiles/Labs/05/az104-05-vnetvm-loop-template.json)
Use Azure PowerShell to deploy the template.
Task 2: Configure local and global virtual network peering.
Create a local virtual network peering between the two virtual networks in the same region.
Create a global virtual network peering between virtual networks in different regions.
Task 3: Test intersite connectivity between virtual machines on the three virtual networks.
Test the virtual machine connections in the same region.
Test the virtual machine connections in different regions.


task 1

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/be00dd19-f50c-4e31-bf21-e79480baa058)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8a202d38-3377-47d0-b4ea-20b55bd48ed0)


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8929f40c-751a-47be-9f22-c0c64ee55663)

upload this file -> https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/blob/master/Allfiles/Labs/05/az104-05-vnetvm-loop-template.json

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/14c2226a-4ffb-42cc-8c39-7d153413b166)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e1961de8-9053-4b88-bfb3-f8dba7a39d62)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1ccec05e-a3dc-46f1-abe4-bb89a74ac2e6)

open the loop-parameters file in the editor  anc change the password

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4d76241d-6347-4342-9f74-034d5658bdd3)

save the change clicking on the more

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f94449fa-64cf-4e7f-a6cd-23bb2fcd5e16)

run the command to deploy the resource group that will be hosting the environment

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9c8baabf-e071-428f-95ba-49179ebbc6ea)

create the 3 virtual networks and deploy virtual machines into them by using the template and parameter files that were uploaded

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/be761294-509f-46ec-9071-2527ca0ae3ed)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/874e426b-7ada-4ecd-9ff6-952887bc03ab)

then close on the X cross top right

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/b741698e-f115-4d0b-bfd7-7a28b1fc6803)


configure local and global virtual network peering

As we used the files to create the virtual networks, it will show 3 VN

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9586745c-f40a-4b2b-8227-2f4ccdf67769)

select the vnet0 > peerings > add

create peering link vnet0 to vnet1

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/43fbe1e1-a26a-43a7-bd9c-ea81d9132427)

create peering link vnet1 to vnet0, select block traffic, then ADD

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6be9a97a-3f27-47f7-b828-a2207ad52a40)


the global peering between eastUS and virtual network in the WestUS
create peering link vnet0 to vnet2
create peering link vnet2 to vnet0

follow the same process above, then you will have 2 link on the vnet0

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d677ca64-bd82-43cd-af63-924c21025885)

back to Virtual networks, select vnet1

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/29c2d0e3-0d6a-4229-b674-da4b9c4809ec)

create peering link vnet1 to vnet2
create peering link vnet2 to vnet1

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/412fe72a-afcc-4734-8350-dcba672a3574)


TEST CONNECTIVITY

go to virtual machines, select the vm0, then select RDP

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f48e6be1-c932-46d8-81af-ce36f26e4ade)

then powershell run as admin, then run the command to test connectivity in the same region

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7e00aaeb-cffe-492d-a5ca-242a6184b361)

test the connectivity in the other azure region

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fc388482-fcb0-4336-80c7-450b8476c6a7)









































