
# Data Structures and Algorithms II – C950

## WGUPS Routing Program

<p align="center">
<img src="https://i.imgur.com/VRWMQxw.png" height="80%" width="80%" alt="Frankie Grande, Ariana Grande et al. are posing for a picture"/>
</p>


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


Plan IP addressing

Private IP addresses enable communication within an Azure virtual network and your on-premises network. You create a private IP address for your resource when you use a VPN gateway or Azure ExpressRoute circuit to extend your network to Azure.


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










