


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




============  Manage and control traffic flow in your Azure deployment with routes

 control Azure virtual network traffic by implementing custom routes.
Identify the routing capabilities of an Azure virtual network
Configure routing within a virtual network
Deploy a basic network virtual appliance
Configure routing to send traffic through a network virtual appliance


A virtual network lets you implement a security perimeter around your resources in the cloud. You can control the information that flows in and out of a virtual network. You can also restrict access to allow only the traffic that originates from trusted sources.

 it is recommended adding network protections in the form of network virtual appliances. The cloud infrastructure team must ensure traffic gets properly routed through the virtual appliances and gets inspected for malicious activity.


 ---- Identify routing capabilities of an Azure virtual network
 
To control traffic flow within your virtual network, you must learn the purpose and benefits of custom routes. You must also learn how to configure the routes to direct traffic flow through a network virtual appliance (NVA).

Network traffic in Azure is automatically routed across Azure subnets, virtual networks, and on-premises networks. This routing is controlled by system routes, which are assigned by default to each subnet in a virtual network. With these system routes, any Azure virtual machine that gets deployed into a virtual network can communicate with any other in the network. These virtual machines are also potentially accessible from on-premises through a hybrid network or the internet.

You can't create or delete system routes, but you can override the system routes by adding custom routes to control traffic flow to the next hop.


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1f882ac3-e349-4549-9a41-46e1e0fe90fb)

The Next hop type column shows the network path taken by traffic sent to each address prefix. The path can be one of the following hop types:

Virtual network: A route is created in the address prefix. The prefix represents each address range created at the virtual-network level. If multiple address ranges are specified, multiple routes are created for each address range.
Internet: The default system route 0.0.0.0/0 routes any address range to the internet, unless you override Azure's default route with a custom route.
None: Any traffic routed to this hop type is dropped and doesn't get routed outside the subnet. By default, the following IPv4 private-address prefixes are created: 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16. The prefix 100.64.0.0/10 for a shared address space is also added. None of these address ranges are globally routable.


Within Azure, there are other system routes. Azure will create these routes if the following capabilities are enabled:

Virtual network peering
Service chaining
Virtual network gateway
Virtual network service endpoint

--- Virtual network peering and service chaining

Virtual network peering and service chaining let virtual networks within Azure be connected to one another. With this connection, virtual machines can communicate with each other within the same region or across regions. This communication in turn creates more routes within the default route table

Service chaining lets you override these routes by creating user-defined routes between peered networks.

The following diagram shows two virtual networks with peering configured. The user-defined routes are configured to route traffic through an NVA or an Azure VPN gateway.

 ![image](https://github.com/M4gOo/PROJECTS/assets/57456345/834e477d-3737-4289-8c73-dda267c146f9)


-- Virtual network gateway

Use a virtual network gateway to send encrypted traffic between Azure and on-premises over the internet and to send encrypted traffic between Azure networks. A virtual network gateway contains routing tables and gateway services.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c54d54b1-2851-4afe-a143-2ac0c66f7ad1)


-- Virtual network service endpoint

Virtual network endpoints extend your private address space in Azure by providing a direct connection to your Azure resources. This connection restricts the flow of traffic: your Azure virtual machines can access your storage account directly from the private address space and deny access from a public virtual machine. As you enable service endpoints, Azure creates routes in the route table to direct this traffic.



--------- Custom routes

System routes might make it easy for you to quickly get your environment up and running, but there are many scenarios in which you'll want to more closely control the traffic flow within your network. For example, you might want to route traffic through an NVA or through a firewall. This control is possible with custom routes.

You have two options for implementing custom routes: create a user-defined route, or use Border Gateway Protocol (BGP) to exchange routes between Azure and on-premises networks.

--- User-defined routes

You can use a user-defined route to override the default system routes so traffic can be routed through firewalls or NVAs.

For example, you might have a network with two subnets and want to add a virtual machine in the perimeter network to be used as a firewall. You can create a user-defined route so that traffic passes through the firewall and doesn't go directly between the subnets.

When creating user-defined routes, you can specify these next hop types:

Virtual appliance: A virtual appliance is typically a firewall device used to analyze or filter traffic that is entering or leaving your network. You can specify the private IP address of a NIC attached to a virtual machine so that IP forwarding can be enabled. Or you can provide the private IP address of an internal load balancer.
Virtual network gateway: Use to indicate when you want routes for a specific address to be routed to a virtual network gateway. The virtual network gateway is specified as a VPN for the next hop type.
Virtual network: Use to override the default system route within a virtual network.
Internet: Use to route traffic to a specified address prefix that is routed to the internet.
None: Use to drop traffic sent to a specified address prefix.
With user-defined routes, you can't specify the next hop type VirtualNetworkServiceEndpoint, which indicates virtual network peering.

You can specify a service tag as the address prefix for a user-defined route instead of an explicit IP range. A service tag represents a group of IP address prefixes from a given Azure service. Microsoft manages the address prefixes encompassed by the service tag and automatically updates the service tag as addresses change. Thus minimizing the complexity of frequent updates to user-defined routes and reducing the number of routes you need to create.


--- Border gateway protocol

A network gateway in your on-premises network can exchange routes with a virtual network gateway in Azure by using BGP. BGP is the standard routing protocol that is normally used to exchange routing and information among two or more networks. BGP is used to transfer data and information between different host gateways like on the internet or between autonomous systems.

You'll typically use BGP to advertise on-premises routes to Azure when you're connected to an Azure datacenter through Azure ExpressRoute. You can also configure BGP if you connect to an Azure virtual network by using a VPN site-to-site connection. BGP offers network stability, because routers can quickly change connections to send packets if a connection path goes down.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/bead5222-13d5-4e8b-9ca9-46d6315346b3)


Route selection and priority
If multiple routes are available in a route table, Azure uses the route with the longest prefix match. For example, if a message gets sent to the IP address 10.0.0.2, but two routes are available with the 10.0.0.0/16 and 10.0.0.0/24 prefixes, Azure selects the route with the 10.0.0.0/24 prefix because it's more specific.

The longer the route prefix, the shorter the list of IP addresses available through that prefix. When you use longer prefixes, the routing algorithm can select the intended address more quickly.

You can't configure multiple user-defined routes with the same address prefix.

If there are multiple routes with the same address prefix, Azure selects the route based on the type in the following order of priority:

User-defined routes
BGP routes
System routes



---- lab

As you implement your security strategy, you want to control how network traffic is routed across your Azure infrastructure.

In the following exercise, you use a network virtual appliance (NVA) to help secure and monitor traffic. You want to ensure communication between front-end public servers and internal private servers is always routed through the appliance.

You configure the network so that all traffic flowing from a public subnet to a private subnet will be routed through the NVA. To make this flow happen, you create a custom route for the public subnet to route this traffic to a perimeter-network subnet. Later, you deploy an NVA to the perimeter-network subnet.

In this exercise, you create the route table, custom route, and subnets. You'll then associate the route table with a subnet.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/efe688cd-affd-4c5c-9e2c-34591704a181)


Create a route table and custom route   https://learn.microsoft.com/en-us/azure/virtual-network/tutorial-create-route-table-portal

In Azure Cloud Shell, run the following command to create a route table.
az network route-table create --name publictable --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --disable-bgp-route-propagation false

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/897b29ff-7819-4aac-8efb-0bbf1d165d7d)

Run the following command in Cloud Shell to create a custom route.
az network route-table route create --route-table-name publictable --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --name productionsubnet --address-prefix 10.0.1.0/24 --next-hop-type VirtualAppliance --next-hop-ip-address 10.0.2.4

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/599a26c9-3c9b-4856-9a1e-30ecb04ae565)

Create a virtual network and subnets
 create the vnet virtual network and the three subnets you need: publicsubnet, privatesubnet, and dmzsubnet.
to create the vnet virtual network and the publicsubnet subnet
az network vnet create --name vnet --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --address-prefixes 10.0.0.0/16 --subnet-name publicsubnet --subnet-prefixes 10.0.0.0/24

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7899b4ea-bb02-40fc-86b7-4a72b4b87df6)

to create the privatesubnet subnet.
az network vnet subnet create --name privatesubnet --vnet-name vnet --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --address-prefixes 10.0.1.0/24

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/44aff8a2-bc0b-455d-8bda-095f4aa5b47a)

to create the dmzsubnet subnet.
az network vnet subnet create --name dmzsubnet --vnet-name vnet --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --address-prefixes 10.0.2.0/24

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d0d8071d-31ba-4e27-9d04-3984b12f6b24)

to show all of the subnets in the vnet virtual network.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/283e215d-8ac9-4094-937c-e9557f6a3cbd)


to associate the route table with the publicsubnet subnet.

az network vnet subnet update --name publicsubnet --vnet-name vnet --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --route-table publictable

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/021201b4-09a5-4db4-8422-70a69a855943)


============= What is an NVA?

A network virtual appliance (NVA) is a virtual appliance that consists of various layers like:
a firewall
a WAN optimizer
application-delivery controllers
routers
load balancers
IDS/IPS
proxies

You can deploy NVAs chosen from providers in Azure Marketplace. Such providers include Cisco, Check Point, Barracuda, Sophos, WatchGuard, and SonicWall. You can use an NVA to filter traffic inbound to a virtual network, to block malicious requests, and to block requests made from unexpected resources.

 You want to implement a secure environment that scrutinizes all incoming traffic and blocks unauthorized traffic from passing on to the internal network. You also want to secure both virtual-machine networking and Azure-services networking as part of your company's network-security strategy.

Your goal is to prevent unwanted or unsecured network traffic from reaching key systems.

Network virtual appliances (NVAs) are virtual machines that control the flow of network traffic by controlling routing. You'll typically use them to manage traffic flowing from a perimeter-network environment to other networks or subnets.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/0b11289a-51c1-4b3a-b67e-204b73a2716c)

You can deploy firewall appliances into a virtual network in different configurations. You can put a firewall appliance in a perimeter-network subnet in the virtual network or if you want more control of security, implement a microsegmentation approach.

With the microsegmentation approach, you can create dedicated subnets for the firewall and then deploy web applications and other services in other subnets. All traffic is routed through the firewall and inspected by the NVAs. You'll enable forwarding on the virtual-appliance network interfaces to pass traffic that is accepted by the appropriate subnet.

Microsegmentation lets the firewall inspect all packets at OSI Layer 4 and, for application-aware appliances, Layer 7. When you deploy an NVA to Azure, it acts as a router that forwards requests between subnets on the virtual network.

Some NVAs require multiple network interfaces. One network interface is dedicated to the management network for the appliance. Additional network interfaces manage and control the traffic processing. After you’ve deployed the NVA, you can then configure the appliance to route the traffic through the proper interface.


For most environments, the default system routes already defined by Azure are enough to get the environments up and running. 
In certain cases, you should create a routing table and add custom routes. Examples include:

Access to the internet via on-premises network using forced tunneling
Using virtual appliances to control traffic flow
You can create multiple route tables in Azure. Each route table can be associated with one or more subnets. A subnet can only be associated with one route table.

If traffic is routed through an NVA, the NVA becomes a critical piece of your infrastructure. Any NVA failures will directly affect the ability of your services to communicate. It's important to include a highly available architecture in your NVA deployment.

https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/nva-ha


--- lab

Create an NVA and virtual machines
deploy a network virtual appliance (NVA) to secure and monitor traffic between your front-end public servers and internal private servers.
You configure the appliance to forward IP traffic. If IP forwarding isn't enabled, traffic that is routed through your appliance will never be received by its intended destination servers.
deploy the nva network appliance to the dmzsubnet subnet. Then you enable IP forwarding so that traffic from * and traffic that uses the custom route is sent to the privatesubnet subnet.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ba2fa47f-eeb5-4b0b-9820-79dab1d038ac)

Deploy the network virtual appliance

 to deploy the appliance. Replace <password> with a suitable password of your choice for the azureuser admin account.
az vm create --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --name nva --vnet-name vnet --subnet dmzsubnet --image Ubuntu2204 --admin-username azureuser --admin-password F!!dsjhf23

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3f22c3d2-7761-4a54-a7b2-15f5202c42d7)

Enable IP forwarding for the Azure network interface

IP forwarding for the nva network appliance is enabled. When traffic flows to the NVA but is meant for another target, the NVA will route that traffic to its correct destination.

NICID=$(az vm nic list --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --vm-name nva --query "[].{id:id}" --output tsv)

echo $NICID

 to get the name of the NVA network interface.

 NICNAME=$(az vm nic show --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --vm-name nva --nic $NICID --query "{name:name}" --output tsv)

echo $NICNAME

 to enable IP forwarding for the network interface.

az network nic update --name $NICNAME \
    --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 \
    --ip-forwarding true


Enable IP forwarding in the appliance

 to save the public IP address of the NVA virtual machine to the variable NVAIP

 NVAIP="$(az vm list-ip-addresses --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --name nva --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" --output tsv)"

echo $NVAIP


to enable IP forwarding within the NVA.

ssh -t -o StrictHostKeyChecking=no azureuser@$NVAIP 'sudo sysctl -w net.ipv4.ip_forward=1; exit;'


Create public and private virtual machines

 Open the Cloud Shell editor and create a file named cloud-init.txt.
 code cloud-init.txt

With this configuration, the inetutils-traceroute package is installed when you create a new VM. This package contains the traceroute utility that you'll use later in this exercise.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/bf87697f-edac-4ad8-abb0-becbdff5864a)

ctrl+S -save
ctrl+Q - exit

 run the following command to create the public VM. Replace <password> with a suitable password for the azureuser account.

az vm create --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --name public --vnet-name vnet --subnet publicsubnet --image Ubuntu2204 --admin-username azureuser --no-wait --custom-data cloud-init.txt --admin-password F!!dsjhf23


to create the private VM. Replace <password> with a suitable password.

az vm create --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --name private --vnet-name vnet --subnet privatesubnet --image Ubuntu2204 --admin-username azureuser --no-wait --custom-data cloud-init.txt --admin-password F!!dsjhf24

Run the following Linux watch command to check that the VMs are running. The watch command periodically runs the az vm list command so that you can monitor the progress of the VMs.

watch -d -n 5 "az vm list \
    --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 \
    --show-details \
    --query '[*].{Name:name, ProvisioningState:provisioningState, PowerState:powerState}' \
    --output table"

 A ProvisioningState value of "Succeeded" and a PowerState value of "VM running" indicate a successful deployment. When all three VMs are running, you're ready to move on.
 CTRL+C to stop

 
 to save the public IP address of the public VM to a variable named PUBLICIP.

PUBLICIP="$(az vm list-ip-addresses --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --name public --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" --output tsv)"

echo $PUBLICIP


to save the public IP address of the private VM to a variable named PRIVATEIP.
PRIVATEIP="$(az vm list-ip-addresses --resource-group learn-dae20bed-5cf0-4ad8-9732-d56999fa65d9 --name private --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" --output tsv)"

echo $PRIVATEIP


Test traffic routing through the network virtual appliance

The final steps use the Linux traceroute utility to show how traffic is routed. You'll use the ssh command to run traceroute on each VM. The first test will show the route taken by ICMP packets sent from the public VM to the private VM. The second test will show the route taken by ICMP packets sent from the private VM to the public VM.

the following command to trace the route from public to private. When prompted, enter the password for the azureuser account that you specified earlier.
ssh -t -o StrictHostKeyChecking=no azureuser@$PUBLICIP 'traceroute private --type=icmp; exit'

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/289feeec-d63a-444c-b68e-d6495cab27d5)

Notice that the first hop is to 10.0.2.4. This address is the private IP address of nva. The second hop is to 10.0.1.4, the address of private. In the first exercise, you added this route to the route table and linked the table to the publicsubnet subnet. So now all traffic from public to private is routed through the NVA.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/435d50ae-9a68-44c8-9674-83ff7be999bd)

to trace the route from private to public. When prompted, enter the password for the azureuser account.
You should see the traffic go directly to public (10.0.0.4) and not through the NVA

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3e1628f1-d6be-45e1-9253-375c8f8a126a)

The private VM is using default routes, and traffic is routed directly between the subnets.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/bbcb78da-ee1b-4574-a673-944ada76e5b4)




====================  Host your domain on Azure DNS

https://learn.microsoft.com/en-us/azure/dns/private-dns-getstarted-portal
https://learn.microsoft.com/en-us/azure/dns/dns-zones-records

Azure DNS lets you host your DNS records for your domains on Azure infrastructure. With Azure DNS, you can use the same credentials, APIs, tools, and billing as your other Azure services.

Let's say that your company recently bought the custom domain name wideworldimporters.com from a third-party domain-name registrar. The domain name is for a new website that your organization plans to launch. You need a hosting service for DNS domains. This hosting service would resolve the wideworldimporters.com domain to your web server's IP address.

IP addresses enable computers and network devices to identify and route requests among themselves.

A DNS server is also known as a DNS name server, or just a name server.

A DNS server carries out one of two primary functions:

Maintains a local cache of recently accessed or used domain names and their IP addresses. This cache provides a faster response to a local domain lookup request. If the DNS server can't find the requested domain, it passes the request to another DNS server. This process repeats at each DNS server until either a match is made or the search times out.
Maintains the key-value pair database of IP addresses and any host or subdomain over which the DNS server has authority. This function is often associated with mail, web, and other internet domain services.

When you connect by using your on-premises network, the DNS settings come from your server. When you connect by using an external location like a hotel, the DNS settings come from the internet service provider (ISP).

Many network devices are now provisioned with both an IPv4 and an IPv6 address. The DNS name server can resolve domain names to both IPv4 and IPv6 addresses.

Whether the DNS server for your domain is hosted by a third party or managed in-house, you'll need to configure it for each host type you're using. Host types include web, email, or other services you're using.


Configuration information for your DNS server is stored as a file within a zone on your DNS server. Each file is called a record. The following record types are the most commonly created and used:

A is the host record, and is the most common type of DNS record. It maps the domain or host name to the IP address.
CNAME is a Canonical Name record that's used to create an alias from one domain name to another domain name. If you had different domain names that all accessed the same website, you'd use CNAME.
MX is the mail exchange record. It maps mail requests to your mail server, whether hosted on-premises or in the cloud.
TXT is the text record. It's used to associate text strings with a domain name. Azure and Microsoft 365 use TXT records to verify domain ownership.
Additionally, there are the following record types:

Wildcards
CAA (certificate authority)
NS (name server)
SOA (start of authority)
SPF (sender policy framework)
SRV (server locations)
The SOA and NS records are created automatically when you create a DNS zone by using Azure DNS.

Some record types support the concept of record sets, or resource record sets. A record set allows for multiple resources to be defined in a single record. For example, here's an A record that has one domain with two IP addresses:
www.wideworldimports.com.     3600    IN    A    127.0.0.1
www.wideworldimports.com.     3600    IN    A    127.0.0.2
SOA and CNAME records can't contain record sets.


Azure DNS acts as the SOA for the domain.
You can't use Azure DNS to register a domain name; you need to use a third-party domain registrar for that.

Azure DNS provides the following security features:

Security Features
Role-based access control, which gives you fine-grained control over users' access to Azure resources. You can monitor their usage and control the resources and services to which they have access.
Activity logs, which let you track changes to a resource and pinpoint where faults occurred.
Resource locking, which gives you a greater level of control to restrict or remove access to resource groups, subscriptions, or any Azure resources.


you can set up an alias record to direct traffic to an Azure public IP address, an Azure Traffic Manager profile, or an Azure Content Delivery Network endpoint.
The alias record set is supported in the following DNS record types:
A
AAAA
CNAME


----------- lab

Configure a public DNS zone
You'll use a DNS zone to host the DNS records for a domain, such as wideworldimports.com.

Step 1: Create a DNS zone in Azure
You used a third-party domain-name registrar to register the wideworldimports.com domain. The domain doesn't point to your organization's website yet.

To host the domain name with Azure DNS, you first need to create a DNS zone for that domain. A DNS zone holds all the DNS entries for your domain.

When creating a DNS zone, you need to supply the following details:

Subscription: The subscription to be used.

Resource group: The name of the resource group to hold your domains. If one doesn't exist, create one to allow for better control and management.

Name: Your domain name, which in this case is wideworldimports.com.

Resource group location: The location defaults to the location of the resource group.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/984f8fef-b8f2-4072-b57c-b12006134d3c)


Get your Azure DNS name servers

After you create a DNS zone for the domain, you need to get the name server details from the name servers (NS) record. You'll use these details to update your domain registrar's information and point to the Azure DNS zone.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fc8435ce-3043-47e6-8686-df37379f1a27)



Update the domain registrar setting
As the domain owner, you need to sign in to the domain-management application provided by your domain registrar. In the management application, edit the NS record and change the NS details to match your Azure DNS name server details.

Changing the NS details is called domain delegation. When you delegate the domain, you must use all four name servers provided by Azure DNS.


Verify delegation of domain name services
The next step is to verify that the delegated domain now points to the Azure DNS zone you created for the domain. This can take as few as 10 minutes, but might take longer.

To verify the success of the domain delegation, query the start of authority (SOA) record. The SOA record was automatically created when the Azure DNS zone was set up. You can do this by using a third-party tool like nslookup.

The SOA record represents your domain and will become the reference point when other DNS servers are searching for your domain on the internet.

To verify the delegation, use nslookup like this:

nslookup -type=SOA wideworldimports.com


Configure your custom DNS settings
The domain name is wideworldimports.com. When it's used in a browser, the domain resolves to your website. But what if you want to add in web servers or load balancers? These resources need to have their own custom settings in the DNS zone, either as an A record or a CNAME.

A record
Each A record requires the following details:

Name: The name of the custom domain, for example webserver1.
Type: In this instance, it's A.
TTL: Represents the "time-to-live" as a whole unit, where 1 is one second. This value indicates how long the A record lives in a DNS cache before it expires.
IP address: The IP address of the server to which this A record should resolve.
CNAME record
The CNAME is the canonical name, or the alias for an A record. Use CNAME when you have different domain names that all access the same website. For example, you might need a CNAME in the wideworldimports zone if you want both www.wideworldimports.com and wideworldimports.com to resolve to the same IP address.

You'd create the CNAME record in the wideworldimports zone with the following information:

NAME: www
TTL: 600 seconds
Record type: CNAME
If you exposed a web function, you'd create a CNAME record that resolves to the Azure function.



------- Configure private DNS zone

To provide name resolution for virtual machines (VMs) within a virtual network and between virtual networks, create a private DNS zone

Step 1: Create private DNS zone

In the Azure portal, search for private DNS zones. To create the private zone, you need enter a resource group and the name of the zone. For example, the name might be something like private.wideworldimports.com

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e084ce9c-e292-466b-af05-61c47cd3c4ec)


Step 2: Identify virtual networks
Let's assume that your organization has already created your VMs and virtual networks in a production environment. Identify the virtual networks associated with VMs that need name-resolution support. To link the virtual networks to the private zone, you need the virtual network names.

Step 3: Link your virtual network to a private DNS zone
To link the private DNS zone to a virtual network, you'll create a virtual network link. In the Azure portal, go to the private zone, and select Virtual network links.


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e21d9b91-a9a2-48b7-9f72-b7e0bdd789b0)


Select Add to pick the virtual network you want to link to the private zone.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ba33f5d3-5379-4a84-a8f9-9238d686fe90)

You add a virtual network link record for each virtual network that needs private name-resolution support.


----- Dynamically resolve resource name by using alias record

You've now successfully delegated the domain from the domain registrar to your Azure DNS and configured an A record to link the domain to your web server.

The next phase of the deployment is to improve resiliency by using a load balancer. Load balancers distribute inbound data requests and traffic across one or more servers. They reduce the load on any one server and improve performance.

You know that the A record and CNAME record don't support direct connection to Azure resources like your load balancers. You've been tasked with finding out how to link the apex domain with a load balancer.

The apex domain is your domain's highest level. In our case, that's wideworldimports.com. The apex domain is also sometimes referred to as the zone apex or root apex. It's often represented by the @ symbol in your DNS zone records.

If you check the DNS zone for wideworldimports.com, you'll see there are two apex domain records: NS and SOA. The NS and SOA records are automatically created when you created the DNS zone.

CNAME records that you might need for an Azure Traffic Manager profile or Azure Content Delivery Network endpoints aren't supported at the zone apex level. However, other alias records are supported at the zone apex level.

Azure alias records enable a zone apex domain to reference other Azure resources from the DNS zone. You don't need to create complex redirection policies. You can also use an Azure alias to route all traffic through Traffic Manager.

The Azure alias record can point to the following Azure resources:

A Traffic Manager profile
Azure Content Delivery Network endpoints
A public IP resource
A front-door profile
Alias records provide lifecycle tracking of target resources, ensuring that changes to any target resource are automatically applied to the DNS zone. Alias records also provide support for load-balanced applications in the zone apex.

The alias record set supports the following DNS zone record types:

A: The IPv4 domain name-mapping record.
AAAA: The IPv6 domain name-mapping record.
CNAME: The alias for your domain, which links to the A record.

Uses for alias records
The following are some of the advantages of using alias records:

Prevents dangling DNS records: A dangling DNS record occurs when the DNS zone records aren't up to date with changes to IP addresses. Alias records prevent dangling references by tightly coupling the lifecycle of a DNS record with an Azure resource.
Updates DNS record set automatically when IP addresses change: When the underlying IP address of a resource, service, or application is changed, the alias record ensures that any associated DNS records are automatically refreshed.
Hosts load-balanced applications at the zone apex: Alias records allow for zone apex resource routing to Traffic Manager.
Points zone apex to Azure Content Delivery Network endpoints: With alias records, you can now directly reference your Azure Content Delivery Network instance.
An alias record allows you to link the zone apex (wideworldimports.com) to a load balancer. It creates a link to the Azure resource rather than a direct IP-based connection. So, if the IP address of your load balancer changes, the zone apex record continues to work.

----- lab

Create alias records for Azure DNS

Your new website's deployment was a huge success. Usage volumes are much higher than anticipated. The single web server on which the website runs is showing signs of strain. Your organization wants to increase the number of servers and distribute the load using a load balancer.

You now know you can use an Azure alias record to provide a dynamic, automatically refreshing link between the zone apex and the load balancer.

Set up a virtual network, load balancer, and VMs in Azure

Manually creating a virtual network, load balancer, and two VMs will take some time. To reduce this time, you can use a Bash setup script that's available on GitHub. Follow these instructions to create a test environment for your alias record.


In Azure Cloud Shell, run the following setup script:

git clone https://github.com/MicrosoftDocs/mslearn-host-domain-azure-dns.git

To run the setup script, run the following commands:
cd mslearn-host-domain-azure-dns
chmod +x setup.sh
./setup.sh

 The script:

Creates a network security group.
Creates two network interface controllers (NICs) and two VMs.
Creates a virtual network and assigns the VMs.
Creates a public IP address and updates the configuration of the VMs.
Creates a load balancer that references the VMs, including rules for the load balancer.
Links the NICs to the load balancer.
After the script completes, it shows you the public IP address for the load balancer. Copy the IP address to use it later.



Create an alias record in your zone apex
Now that you've created a test environment, you're ready to set up the Azure alias record in your zone apex.

In the Azure portal, select Resource groups. The Resource groups pane appears.

Select the resource group: learn-dbe8cdc0-bcfc-4543-bbbd-ce5cd62047eb. The Resource group pane appears.

In the list of resources, select the DNS zone you created in the previous exercise, wideworldimportsXXXX.com. The wideworldimportsXXXX.com DNS zone pane appears.

In the menu bar, select + Record set. The Add record set pane appears.

Enter the following values for each setting to create an alias record.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e6ded4d7-74c8-4e28-bec4-1ce793f20eaa)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/701c71cb-31b4-43f8-b99c-34a21894ff8e)

When the new alias record is created, it should look something like this:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7799b038-a51d-4fa9-a179-ebfe5c6a64c4)

Verify that the alias resolves to the load balancer
Now, you need to verify that the alias record is set up correctly. In a real-world scenario, you'd have an actual domain, and would've completed the domain delegation to Azure DNS. You'd use the registered domain name for this exercise. Because this unit assumes there's no registered domain, you'll use the public IP address.

In the Azure portal, go to the resource group, select myPublicIP, then select the Copy icon next to the IP address

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/01b067e2-31c8-4cd0-a060-4c49a268d3d6)

In a web browser, paste the Public IP address as the URL.

You'll see a basic web page that shows the name of the VM to which the load balancer sent the request.



===================== Configure network security groups

virtual networks to enable resources to communicate with other resources, over the internet, and with on-premises networks. To provide secure communication and control access within a virtual network, you can use network security groups and network security group rules.
You need to secure both virtual machine networking and Azure services networking. 

You can limit network traffic to resources in your virtual network by using a network security group. You can assign a network security group to a subnet or a network interface, and define security rules in the group to control network traffic.

A network security group contains a list of security rules that allow or deny inbound or outbound network traffic.
A network security group can be associated to a subnet or a network interface.
A network security group can be associated multiple times.
You create a network security group and define security rules in the Azure portal.

Network security groups are defined for your virtual machines in the Azure portal. The Overview page for a virtual machine provides information about the associated network security groups. You can see details such as the assigned subnets, assigned network interfaces, and the defined security rules.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7f5cb06d-2ae4-4c95-89c5-f840327590a4)

You can assign network security groups to a subnet and create a protected screened subnet (also referred to as a demilitarized zone or DMZ). A DMZ acts as a buffer between resources within your virtual network and the internet.

Use the network security group to restrict traffic flow to all machines that reside within the subnet.
Each subnet can have a maximum of one associated network security group.

You can assign network security groups to a network interface card (NIC).

Define network security group rules to control all traffic that flows through a NIC.
Each network interface that exists in a subnet can have zero, or one, associated network security groups.


You can define rules to control the traffic flow in and out of virtual network subnets and network interfaces.

Azure creates several default security rules within each network security group, including inbound traffic and outbound traffic. Examples of default rules include DenyAllInbound traffic and AllowInternetOutbound traffic.
Azure creates the default security rules in each network security group that you create.
You can add more security rules to a network security group by specifying conditions for any of the following settings:

Name
Priority
Port
Protocol (Any, TCP, UDP)
Source (Any, IP addresses, Service tag)
Destination (Any, IP addresses, Virtual network)
Action (Allow or Deny)
Each security rule is assigned a Priority value. All security rules for a network security group are processed in priority order. When a rule has a low Priority value, the rule has a higher priority or precedence in terms of order processing.

You can't remove the default security rules.

You can override a default security rule by creating another security rule that has a higher Priority setting for your network security group.

Azure defines three default inbound security rules for your network security group. These rules deny all inbound traffic except traffic from your virtual network and Azure load balancers. The following image shows the default inbound security rules for a network security group in the Azure portal.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/57432176-956c-4fa5-840c-7495a18fed13)

Azure defines three default outbound security rules for your network security group. These rules only allow outbound traffic to the internet and your virtual network. The following image shows the default outbound security rules for a network security group in the Azure portal.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2cc6999e-53f7-46d9-aa99-6c4665b8a495)


Each network security group and its defined security rules are evaluated independently. Azure processes the conditions in each rule defined for each virtual machine in your configuration.

For inbound traffic, Azure first processes network security group security rules for any associated subnets and then any associated network interfaces.
For outbound traffic, the process is reversed. Azure first evaluates network security group security rules for any associated network interfaces followed by any associated subnets.
For both the inbound and outbound evaluation process, Azure also checks how to apply the rules for intra-subnet traffic.
How Azure ends up applying your defined security rules for a virtual machine determines the overall effectiveness of your rules.

Consider the following virtual network configuration that shows network security groups (NSGs) controlling traffic to virtual machines (VMs). The configuration requires security rules to manage network traffic to and from the internet over TCP port 80 via the network interface.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f7069952-b65f-40bf-9d97-8ffec1232a4a)

In this virtual network configuration, there are three subnets. Subnet 1 contains two virtual machines: VM 1 and VM 2. Subnet 2 and Subnet 3 each contain one virtual machine: VM 3 and VM 4, respectively. Each VM has a network interface card (NIC).

Azure evaluates each NSG configuration to determine the effective security rules:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5fa61a2e-eb41-4466-9a2f-594ceaeb1364)

Azure processes rules for inbound traffic for all VMs in the configuration. Azure identifies if the VMs are members of an NSG, and if they have an associated subnet or NIC.

When an NSG is created, Azure creates the default security rule DenyAllInbound for the group. The default behavior is to deny all inbound traffic from the internet. If an NSG has a subnet or NIC, the rules for the subnet or NIC can override the default Azure security rules.

NSG inbound rules for a subnet in a VM take precedence over NSG inbound rules for a NIC in the same VM.


Azure processes rules for outbound traffic by first examining NSG associations for NICs in all VMs.

When an NSG is created, Azure creates the default security rule AllowInternetOutbound for the group. The default behavior is to allow all outbound traffic to the internet. If an NSG has a subnet or NIC, the rules for the subnet or NIC can override the default Azure security rules.

NSG outbound rules for a NIC in a VM take precedence over NSG outbound rules for a subnet in the same VM.

Review the following considerations regarding creating effective security rules for machines in your virtual network.

Consider allowing all traffic. If you place your virtual machine within a subnet or utilize a network interface, you don't have to associate the subnet or NIC with a network security group. This approach allows all network traffic through the subnet or NIC according to the default Azure security rules. If you're not concerned about controlling traffic to your resource at a specific level, then don't associate your resource at that level to a network security group.

Consider importance of allow rules. When you create a network security group, you must define an allow rule for both the subnet and network interface in the group to ensure traffic can get through. If you have a subnet or NIC in your network security group, you must define an allow rule at each level. Otherwise, the traffic is denied for any level that doesn't provide an allow rule definition.

Consider intra-subnet traffic. The security rules for a network security group that's associated to a subnet can affect traffic between all virtual machines in the subnet. By default, Azure allows virtual machines in the same subnet to send traffic to each other (referred to as intra-subnet traffic). You can prohibit intra-subnet traffic by defining a rule in the network security group to deny all inbound and outbound traffic. This rule prevents all virtual machines in your subnet from communicating with each other.

Consider rule priority. The security rules for a network security group are processed in priority order. To ensure a particular security rule is always processed, assign the lowest possible priority value to the rule. It's a good practice to leave gaps in your priority numbering, such as 100, 200, 300, and so. The gaps in the numbering allow you to add new rules without having to edit existing rules.


If you have several network security groups and aren't sure which security rules are being applied, you can use the Effective security rules link in the Azure portal. You can use the link to verify which security rules are applied to your machines, subnets, and network interfaces.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2cf7e4f9-8847-434c-883e-7f50d9353017)


-------- Create network security group rules

It's easy to add security rules to control inbound and outbound traffic in the Azure portal. You can configure your virtual network security group rule settings, and select from a large variety of communication services, including HTTPS, RDP, FTP, and DNS.

Source: Identifies how the security rule controls inbound traffic. The value specifies a specific source IP address range that's allowed or denied. The source filter can be any resource, an IP address range, an application security group, or a default tag.

Destination: Identifies how the security rule controls outbound traffic. The value specifies a specific destination IP address range that's allowed or denied. The destination filter value is similar to the source filter. The value can be any resource, an IP address range, an application security group, or a default tag.

Service: Specifies the destination protocol and port range for the security rule. You can choose a predefined service like RDP or SSH or provide a custom port range. There are a large number of services to select from.

Priority: Assigns the priority order value for the security rule. Rules are processed according to the priority order of all rules for a network security group, including a subnet and network interface. The lower the priority value, the higher priority for the rule.

***Service tags represent a group of IP addresses. Other service tags are Internet, SQL, Storage, AzureLoadBalancer, and AzureTrafficManager.

You can implement application security groups in your Azure virtual network to logically group your virtual machines by workload. You can then define your network security group rules based on your application security groups.
https://learn.microsoft.com/en-us/azure/virtual-network/application-security-groups

Application security groups work in the same way as network security groups, but they provide an application-centric way of looking at your infrastructure. You join your virtual machines to an application security group. Then you use the application security group as a source or destination in the network security group rules.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/500bd39b-5d78-4837-872a-5c03cf0bfee9)

Here are the scenario requirements for our example configuration:

We have six virtual machines in our configuration with two web servers and two database servers.
Customers access the online catalog hosted on our web servers.
The web servers must be accessible from the internet over HTTP port 80 and HTTPS port 443.
Inventory information is stored on our database servers.
The database servers must be accessible over HTTPS port 1433.
Only our web servers should have access to our database servers.

Solution
For our scenario, we need to build the following configuration:

Create application security groups for the virtual machines.
Create an application security group named WebASG to group our web server machines.
Create an application security group named DBASG to group our database server machines.
Assign the network interfaces for the virtual machines.
For each virtual machine server, assign its NIC to the appropriate application security group.
Create the network security group and security rules.
Rule 1: Set Priority to 100. Allow access from the internet to machines in the WebASG group from HTTP port 80 and HTTPS port 443.
Rule 1 has the lowest priority value, so it has precedence over the other rules in the group. Customer access to our online catalog is paramount in our design.
Rule 2: Set Priority to 110. Allow access from machines in the WebASG group to machines in the DBASG group over HTTPS port 1433.
Rule 3: Set Priority to 120. Deny (X) access from anywhere to machines in the DBASG group over HTTPS port 1433.
The combination of Rule 2 and Rule 3 ensures that only our web servers can access our database servers. This security configuration protects our inventory databases from outside attack.


There are several advantages to implementing application security groups in your virtual networks.

>Consider IP address maintenance. When you control network traffic by using application security groups, you don't need to configure inbound and outbound traffic for specific IP addresses. If you have many virtual machines in your configuration, it can be difficult to specify all of the affected IP addresses. As you maintain your configuration, the number of your servers can change. These changes can require you to modify how you support different IP addresses in your security rules.
>Consider no subnets. By organizing your virtual machines into application security groups, you don't need to also distribute your servers across specific subnets. You can arrange your servers by application and purpose to achieve logical groupings.
>Consider simplified rules. Application security groups help to eliminate the need for multiple rule sets. You don't need to create a separate rule for each virtual machine. You can dynamically apply new rules to designated application security groups. New security rules are automatically applied to all the virtual machines in the specified application security group.
>Consider workload support. A configuration that implements application security groups is easy to maintain and understand because the organization is based on workload usage. Application security groups provide logical arrangements for your applications, services, data storage, and workloads.

----- lab   https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview  go to the tab How-to guides
https://learn.microsoft.com/en-us/azure/virtual-network/tutorial-filter-network-traffic

our organization wants to ensure that access to virtual machines is restricted. As the Azure Administrator, you need to:

Create and configure network security groups.
Associate network security groups to virtual machines.
Deny and allow access to the virtual machines by using network security groups.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e288b81b-bd5d-45d3-98d8-42f8e65d5902)

Objectives
Task 1: Create a virtual machine to test network security.
Create a Windows Server virtual machine.
Don't configure any inbound port rules or NIC network security groups.
Verify the virtual machine is created.
Review the Inbound port rules tab, and note there are no network security groups associated with the virtual machine.
Task 2: Create a network security group, and associate the group with the virtual machine.
Create a network security group.
Associate the network security group with the virtual machine network interface (NIC).
Task 3: Configure an inbound security port rule to allow RDP.
Verify you can't connect to the virtual machine by using RDP.
Add an inbound port rule to allow RDP to the virtual machine on port 3389.
Verify you can now connect to the virtual machine with RDP.
Task 4: Configure an outbound security port rule to deny internet access
Verify you can access the internet from the virtual machine.
Add an outbound port rule to deny internet access from the virtual machine.
Verify you can no longer access the internet from the virtual machine.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8c9f7550-d83d-4d13-96be-7ac2d30e7c82)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/599d2bc4-bd3d-4990-94f7-b080ee0b2dec)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9f16b3fc-58b7-4c32-9f0d-ff267a0e4122)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6e7aba4e-24bd-4783-8f37-1c3a7c0c3ebc)

review the inboud port rules tad, no network security group associatedd with the network interface of the virtual machine ofr the subnet to which the network interface is attached.
remember the name of the network interface to create network security group.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/db760645-1cee-42ff-8101-bfe457c476e1)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4f266acf-1be0-4cb6-8c23-448a0ad2ed33)

Click on Resources > Network Interfaces > Associate > drop-down choose the network interface identified previous task 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d1300c70-c833-4158-aae0-faf35c4af10c)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e592783a-3921-4c3c-af55-217bd2a61e9e)


Now lets configure an inbound security port rule to allow RDP

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e7995312-1e39-490d-86ff-e1579043bbbd)

Then select Connect > Download RDP file > open the file > connect it will give an error

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d1614db1-4650-4f96-9be6-4647d52854d5)

back to the virutal machine, select networking, then add inbound port rule, for RDP is TCP/3389, set the priority as well

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f3a01b2d-2b2c-480f-92f7-f3deeabeae63)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2d7cf923-823b-409c-a554-cee8c05d9436)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5ec47349-aaf7-4da9-8577-fd6257444f66)


Now lets configure an outbound security port rule to deny internet access

back to the virutal machine (you can leave running the VM), select networking, then add outbound port rule, 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ae760b0f-9010-43f9-b244-da7f0814d760)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3dbf666c-713d-466a-8042-79878100591e)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a62520bd-cdcb-42df-8abf-7862603c4c00)






------------ vocabulary

Resource Group
 They are logical containers for a collection of resources that can be treated as one logical instance: You can use resource groups to control all of their members collectively.

 ![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a9e64cf9-1e3c-4668-a013-c52aab762880)
































































































































































































