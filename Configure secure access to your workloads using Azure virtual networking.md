


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





































