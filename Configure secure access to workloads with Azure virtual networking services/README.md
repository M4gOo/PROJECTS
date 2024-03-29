

<h2>Requeriments:</h2>

- [Provide network isolation and segmentation for the web application.](#Provide-network-isolation-and-segmentation-for-the-web-application)
- [Control the network traffic to and from the web application.](#Control-network-traffic-to-and-from-the-web-application)
- [Protect the web application from malicious traffic and block unauthorized access.](#Protect-the-web-application-from-malicious-traffic-and-block-unauthorized-access)
- [Route traffic to the firewall.](#Route-traffic-to-the-firewall)
- [Record and resolve domain names internally.](#Record-and-resolve-domain-names-internally)


<h2>What need to be done:</h2>

- <b>Create and configure virtual networks</b>
- <b>Create and configure network security groups (NSGs) and application security group (ASGs)</b>
- <b>Create and configure Azure Firewall</b>
- <b>Configure network routing</b>
- <b>Create DNS zones and configure DNS settings</b>

<h2>Documentation:</h2>

https://learn.microsoft.com/en-us/azure/networking/fundamentals/networking-overview


<h1>Provide network isolation and segmentation for the web application</h1>

First I create a Resource Group called RG, then I moved to create Virtual Networks, Subnets and the Peering. Below shows the architecture.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1a0c9c84-9e97-45b6-a405-dc209b1cc036)

Creating an Virtual Network called app-vnet and the subnets.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fd720a38-8f22-47ec-b528-3a342a5fd8cd)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1db4abf6-304b-416c-996c-95b942c635b7)


Creating an Virtual Network called shared-services-vnet and the subnet.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/42f82450-dc83-4a4d-a3b2-39ef5ea873d0)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6a9c0a11-411f-4716-9c70-d07de7ca2a1b)


Confirmed both virtual networks have deployed.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2c1cbf27-e316-4bda-8cdf-6b090c2e5f62)

Next step was Peering the two virtual networks to allow traffic to flow in both directions securely and privately between the app-vnet and shared-services-vnet virtual networks.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9d8d94ea-424a-4df2-82d3-ca2ec8da9264)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/71b596b2-671f-43d2-8ed7-55379bdf3dd8)


<h1>Control network traffic to and from the web application</h1>

To enhance the security of the web application, network security groups (NSG) and application security groups (ASG) can be configured. 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/23d66514-9392-4130-b6a9-b6f865573d7b)

To-do list:
- Create an NSG.
- Create NSG rules.
- Associate an NSG to a subnet.
- Create and use Application Security Groups in NSG rules.


Creating an application security group first.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1fb428e3-63ee-495a-aa79-2bfd39ac6696)

Then create an network security group. 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/96597d00-fa9f-4fbc-a205-9a1cd87a88bb)

Next is associate the network security group named ***app-vnet-nsg*** to the ***backend subnet*** in the virtual network named ***app-vnet***. 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/86424889-0f1c-4fe0-a36a-8e8962197142)

To have access to the ASG it is necessary to add an inbound security rule, in this case named AllowSSH in the app-vnet-nsg network security group to allow incoming TCP traffic on port 22 to reach the app-backend-asg application security group. 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/61b9f4a8-daf0-4dbd-8460-a9496f76b5d8)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e2501fcd-1f26-44a2-9c6f-b9e37a07cbf9)

Now, it is necessary to create some VM, I am using Cloud Shell to create the VMs from the [GitHub Repo](https://github.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/blob/main/Instructions/Labs/azuredeploy.json) 

The ARM template used to create VMs:

```
$RGName = "RG1"
New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateUri https://raw.githubusercontent.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/main/Instructions/Labs/azuredeploy.json
```

Successfully created 
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1c05d927-40f8-4bfc-8665-f0b0e5b20955)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/880ec11f-3ff6-4b5b-8d61-b4effd33f9c3)

After VMs created, I associate the app-backend-asg application security group to the VM2-nic network interface that is attached to VM2

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5561c5c1-f39c-4d24-b84b-ac2f7d547019)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/84b9a4df-084e-487e-bb99-9eb5038c48fc)


<h2>Protect the web application from malicious traffic and block unauthorized access</h2>

In addition to NSG and ASG, a firewall can be configured to add an extra layer of security to the web application. A firewall protects the web application from malicious traffic and blocks unauthorized access with policies you configure.


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ba37c883-32c6-4c43-ad2f-790b34d2294f)

To-do list:
- Create an Azure Firewall.
- Create and configure a firewall policy
- Create an application rule collection.
- Create a network rule collection.

First, I created a subnet for firewall in app-vnet

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/0fbd3636-8640-4381-84b0-d0d68fca8292)

Secondly, I created a firewall using those values below:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/bbcd3a8e-4707-4607-9efe-b859fdc5a87c)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d00c73ca-f6e7-4643-81df-62d3f419a8ee)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e0562d42-54f7-42e1-97c8-c9d9131abb75)

Next, create an [application rule](https://learn.microsoft.com/pt-br/azure/firewall/tutorial-firewall-deploy-portal#configure-an-application-rule) collection inside the fw-policy that contains a single Target FQDN rule by using the values in the following table. 

I can use an application rule to filter traffic based on an FQDN address such as server1.database.windows.net.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8723e621-35b4-4f49-aabc-e2d36fc47d22)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/083758cb-2bd6-4be6-b5fd-928252d56bc7)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/772de25e-7558-4dab-b7f6-3c7a1116e9aa)

Create a [network rule](https://learn.microsoft.com/pt-br/azure/firewall/tutorial-firewall-deploy-portal#configure-a-network-rule) collection that contains a single IP Address rule

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d7140cf0-f453-4627-90d2-f119b30750a9)

Verify that the Azure Firewall and Firewall Policy.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3ad61864-49c6-47e7-9cd0-5e05b4ac45fa)


<h2>Route traffic to the firewall</h2>

I need to route network traffic to the firewall subnet so it can filter and inspect the traffic. Route tables provide control over the routing of network traffic to and from the web application. Network Traffic is subject to the firewall rules when you route your network traffic to the firewall as the subnet default gateway.

To-do list:
- Create and configure a [route table](https://learn.microsoft.com/en-us/azure/virtual-network/manage-route-table)
- Link a [route table to a subnet](https://learn.microsoft.com/pt-br/azure/virtual-network/tutorial-create-route-table-portal#associate-a-route-table-to-a-subnet)


Creating a route table named app-vnet-firewall-rt in the RG1 resource group using the East US region.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d9f5df9a-4971-4eae-80a9-13477e2e3db4)

Associating the app-vnet-firewall-rt route table to the frontend and backend subnets in app-vnet.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/52fd1ff7-795a-460a-b1ca-b5af036fd9d8)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/cd993ba1-f3f5-4d1d-85bf-c96043e450d8)


Creating a route in the app-vnet-firewall-rt named outbound-firewall with address prefix 0.0.0.0/0 and Next hop type Virtual Appliance. Using the private IP address of the firewall for the Next hop address.

private IP addr of the firewall
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/b0892bb4-88cc-440f-adb6-0a91b5b0c27f)


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/10c23f4d-463e-429c-aad0-a651a6e5adaa)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/24c6a444-13d6-43e1-95e7-dfb1fa9ac948)

Now the outbound traffic from the front end and backend subnet will route to the firewall.


<h2>Record and resolve domain names internally</h2>

Virtual machines in virtual networks can use domain name instead of IPs for internal communication. In that case, the domain names will be resolved with a private DNS zone through a virtual network link.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/71d5509c-d22a-414a-9637-00538c1cb58a)

To-do list:
- Create and configure a private DNS zone.
- Create and configure DNS records.
- Configure DNS settings on a virtual network.


Creating a Private DNS

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1a3fee11-1b5a-424c-a8c7-c27dbd9811c4)

Creating a Virtual Network Link within the private DNS Zone to the app-vnet named **app-vnet-link** with auto registration enabled.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/93ea068a-2571-4641-9a37-19c78743d902)

Creating a DNS record set for VM2 named backend that is Type A with IP address 10.1.1.4. This IP address can be found at Virtual Machines > VM2 > overview.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/70d66c89-1c80-4f53-8b70-6e57b6eebbb0)

Verify that contoso.com has a record set named backend

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/da221f37-cd49-4e8d-9e21-cf05a74661b1)

All the resources used for this project.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/16605903-81fb-487e-ae1f-dc2c5f219908)















