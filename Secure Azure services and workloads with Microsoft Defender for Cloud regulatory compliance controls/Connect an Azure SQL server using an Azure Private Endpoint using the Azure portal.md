
Securely connecting (protecting sensitive data and maintaining the integrity) an Azure SQL server via Azure Private Endpoint in the Azure portal, enhancing data communication security.

Your organization requires secure and private access to the SQL Server from within the virtual network to ensure compliance with industry regulations. 

A private endpoint is a network interface that uses a private IP address from your virtual network. This network interface connects you privately and securely to a service that's powered by Azure Private Link. 
By enabling a private endpoint, you're bringing the service into your virtual network.

By using an Azure Private Endpoint, you can establish a secure, private connection between the client (such as an application or virtual machine) and the Azure SQL server. This connection ensures that data exchanged between the client and the SQL server remains within the Azure backbone network, enhancing security and isolating it from public internet traffic.

Azure SQL server requires its own Azure Private Endpoint. Private Endpoints are specific to each resource and cannot be shared between multiple SQL servers. This ensures isolation and security between the SQL servers.
 
Azure Private Link is the Azure service used to create and manage Private Endpoints. It allows you to securely connect to Azure services, including Azure SQL server, over a private network connection.

The service could be an Azure service such as Azure Storage, Azure Cosmos DB, Azure SQL Database amd your own service, using Private Link service.

Private endpoints enable connectivity between the customers from the same:

- Virtual network
- Regionally peered virtual networks
- Globally peered virtual networks
- On-premises environments that use VNP or Express Route.
- Services that are powered by Private Link

Private endpoints support network policies. Network policies enable support for Network Security Groups (NSG), User Defined Routes (UDR), and Application Security Groups (ASG).

Azure Private Link enables you to access Azure PaaS Services (for example, Azure Storage and SQL Database) and Azure hosted customer-owned/partner services over a private endpoint in your virtual network.
Traffic between your virtual network and the service travels the Microsoft backbone network. Exposing your service to the public internet is no longer necessary. You can create your own private link service in your virtual network and deliver it to your customers.
Setup and consumption using Azure Private Link is consistent across Azure PaaS, customer-owned, and shared partner services.


# LAB - Deploy a virtual machine to test connectivity privately and securely to the SQL server across the private endpoint

To-do list:
- Create a virtual network and bastion host.
- Create a virtual machine.
- Create an Azure SQL server and private endpoint.
- Test connectivity to the SQL server private endpoint.

Create a resource group and virtual network:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/defddc79-3a24-464a-9a20-478cc4030d07)

Configuration IP address
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3f172ce8-c082-40ad-93e4-38bc252b6e20)

Configure Bastion host. The bastion host will be used to connect securely to the virtual machine for testing the private endpoint.
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7fee9d2c-7a1d-4c76-9928-92e799d853ef)


Create a VM:

At the end select inbound ports None

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/49810c20-db71-41a8-b519-f6076734d429)

on Network tab

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e848e0ad-366e-45e3-95fc-a7868c864545)


Create an Azure SQL server and private endpoint:

Create a resource > Databases > SQL database

in the Basics tab enter those informations:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5ae40d99-3aac-4ab9-a46e-87fcaf649615)

In Create SQL Database Server, enter those information:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/08dd576e-cd4f-42f8-b620-c78f49a6ec53)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3ae43f3d-0876-4d22-a488-e88a00b107bf)


In the Networking tab, enter or select this information:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a4e5c26a-fbfc-4c7e-97b6-bb1ae0baf361)

Select + Add private endpoint in Private endpoints

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5898e9c4-6af1-4588-81ae-27d706a0bad0)


Disable public access to Azure SQL logical server:

In the Azure portal search box, enter mysqlserver whcih was done previously. On the Network select Disable for Public network access

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/281cb5cc-51f0-4905-aca4-ce55908de1fc)


Test connectivity to private endpoint:

Select Resource groups in the left-hand navigation pane

- Select CreateSQLEndpointTutorial
- Select myVM
  - On the overview page for myVM, select Connect then Bastion.
  - Enter the <username> and <password> during the virtual machine creation.
  - Select Connect button.

- Open Windows PowerShell on the server after you connect.
  - Enter nslookup sqlserver-name.database.windows.net. Replace sqlserver-name with the name of the SQL server you created in the previous steps. You’ll receive a message similar to what is displayed below:
```
Server:  UnKnown
Address:  168.63.129.16
   
Non-authoritative answer:
Name:    mysqlserver1a.privatelink.database.windows.net
Address:  10.1.0.5
Aliases:  mysqlserver1a.database.windows.net
```
A private IP address of 10.1.0.5 is returned for the SQL server name. This address is in mySubnet1a subnet of myVNet1a virtual network you created previously.


- Install SQL Server Management Studio on myVM.
- Open SQL Server Management Studio.
- In Connect to server, enter or select this information:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ed3658a9-c2ac-4279-a4ee-ffb3e8f79915)

Select Connect and then browse databases from left menu.

























































