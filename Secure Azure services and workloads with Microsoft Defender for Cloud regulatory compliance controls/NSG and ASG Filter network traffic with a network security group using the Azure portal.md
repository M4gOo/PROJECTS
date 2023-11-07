
The Resource Group stores metadata about the resources. Therefore, when you specify a location for the resource group, you are specifying where that metadata is stored. 
For compliance reasons, you may need to ensure that your data is stored in a particular region.

All resources in a virtual network can communicate outbound to the internet, by default.
You can communicate inbound to a resource by assigning a public IP address or a public load balancer.
You can also use public IP, Network Address Translation (NAT) gateway, or public load balancer to manage your outbound connections.



Azure routes traffic between subnets, connected virtual networks, on-premises networks, and the Internet, by default. You can implement either or both of the following options to override the default routes Azure creates:

Route tables: You can create custom route tables with routes that control where traffic is routed to for each subnet.
Border gateway protocol (BGP) routes: If you connect your virtual network to your on-premises network using an Azure VPN Gateway or ExpressRoute connection, you can propagate your on-premises Border Gateway Protocol (BGP) routes to your virtual networks.



You can filter network traffic between subnets using either or both of the following options:

Network security groups: Network security groups and application security groups can contain multiple inbound and outbound security rules. These rules enable you to filter traffic to and from resources by source and destination IP address, port, and protocol.
Network virtual appliances: A network virtual appliance is a VM that performs a network function, such as a firewall, Wide-Area Network (WAN) optimization, or other network function.

Existing connections may not be interrupted when you remove a security rule that allowed the connection. Modifying network security group rules will only affect new connections. 
When a new rule is created or an existing rule is updated in a network security group, it will only apply to new connections. Existing connections are not reevaluated with the new rules.

Existing connections may not be interrupted when you remove a security rule that allowed the connection. Modifying network security group rules will only affect new connections.
When a new rule is created or an existing rule is updated in a network security group, it will only apply to new connections. Existing connections are not reevaluated with the new rules.


Intra-Subnet traffic
It's important to note that security rules in an NSG associated to a subnet can affect connectivity between VMs within it. 
By default, virtual machines in the same subnet can communicate based on a default NSG rule allowing intra-subnet traffic.
If you add a rule to NSG1 that denies all inbound and outbound traffic, VM1 and VM2 won't be able to communicate with each other.
You can easily view the aggregate rules applied to a network interface by viewing the effective security rules for a network interface. 
You can also use the IP flow verify capability in Azure Network Watcher to determine whether communication is allowed to or from a network interface
TIP: Unless you have a specific reason to, we recommend that you associate a network security group to a subnet, or a network interface, but not both


## ASG

Application security groups enable you to configure network security as a natural extension of an application's structure, 
allowing you to group virtual machines and define network security policies based on those groups. 


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/37791fe4-5cd2-44a1-af5b-2753a389a42c)

Though each network interface (NIC) in this example is a member of only one network security group, a network interface can be a member of multiple application security groups, up to the Azure Limits.
None of the network interfaces have an associated network security group. NSG1 is associated to both subnets

Allow-HTTP-Inbound-Internet

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/27a152b6-f7ab-448e-b19e-337a25e79b45)

This rule is needed to allow traffic from the internet to the web servers. Because inbound traffic from the internet is denied by the DenyAllInbound default security rule, 
no extra rule is needed for the AsgLogic or AsgDb application security groups.


Deny-Database-All

Because the AllowVNetInBound default security rule allows all communication between resources in the same virtual network, this rule is needed to deny traffic from all resources.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3c2c2cbe-f061-4073-b6e2-f20933347e81)


Allow-Database-BusinessLogic

This rule allows traffic from the AsgLogic application security group to the AsgDb application security group. The priority for this rule is higher than the priority for the Deny-Database-All rule.
As a result, this rule is processed before the Deny-Database-All rule, so traffic from the AsgLogic application security group is allowed, whereas all other traffic is blocked.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/32be9385-3d2f-4cf6-be83-01d3b7657da8)


To minimize the number of security rules you need, and the need to change the rules, plan out the application security groups you need and create rules using service tags or application security groups,
rather than individual IP addresses, or ranges of IP addresses, whenever possible.


-------- LAB

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4e82646e-2a09-4786-8b1a-f402403c2617)


I dont need to specify denys, only allows. Configure http(s) for asg-web and RDP for asg-mgmt

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d7509d99-1ce0-4201-b9de-b73003169646)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fe317777-36d3-4aa3-87ff-6f3992a5fe07)

Then create 2 VMs and associate them on every ASG realted

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3406778a-08aa-43b1-bbd4-f5e120d5b14f)








