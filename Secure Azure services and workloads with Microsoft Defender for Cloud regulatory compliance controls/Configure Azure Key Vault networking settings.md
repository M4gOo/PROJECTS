
Configure Azure Key Vault networking settings via the Azure portal, ensuring secure and controlled access to stored secrets (secure the communication between Key Vault and other Azure resources).
A secret is anything that you want to tightly control access to, such as API keys, passwords, certificates, or cryptographic keys. 

Azure Key Vault is a powerful service that allows organizations to securely store and manage cryptographic keys, secrets, and certificates.


Companies stores sensitive data in Azure Key Vault, that organization has strict regulatory requirements and compliance standards to adhere to. That is important to know how to configure Azure Key Vault networking settings 

Key Vault service supports two types of containers: 
- vaults - Vaults support storing software and HSM-backed keys, secrets, and certificates.
- managed hardware security module (HSM) pools - Managed HSM pools only support HSM-backed keys.

To do any operations with Key Vault, you first need to authenticate to it. There are three ways to authenticate to Key Vault:
- Managed identities for Azure resources: When you deploy an app on a virtual machine in Azure, you can assign an identity to your virtual machine that has access to Key Vault. You can also assign identities to other Azure resources. The benefit of this approach is that the app or service isn't managing the rotation of the first secret. Azure automatically rotates the identity. We recommend this approach as a best practice.
- Service principle and certificate: You can use a service principle and an associated certificate that has access to Key Vault. We don't recommend this approach because the application owner or developer must rotate the certificate.
- Service principle and secret: Although you can use a service principle and a secret to authenticate to Key Vault, we don't recommend it. It's hard to automatically rotate the bootstrap secret that's used to authenticate to Key Vault.

Azure Key Vault enforces Transport Layer Security (TLS) protocol to protect data when it’s traveling between Azure Key vault and clients.
TLS provides strong authentication, message privacy, and integrity (enabling detection of message tampering, interception, and forgery), interoperability, algorithm flexibility, and ease of deployment and use.

Purpose of configuring networking settings for Azure Key Vault? To contol access to the Azure Key Vault Instance



### Best Practices

Using a vault per application per environment (development, pre-production, and production), per region. Using separate key vaults helps you not share secrets across environments and regions. It will also reduce the threat in a breach.

Encryption keys and secrets like certificates, connection strings, and passwords are sensitive and business critical. You need to secure access to your key vaults by allowing only authorized applications and users. 
Suggestions for controlling access to your vault are as follows:
- Lock down access to your subscription, resource group, and key vaults using role-based access control.
- Create access policies for every vault.
- Use the principle of least privilege access to grant access.
- Turn on firewall and virtual network service endpoints.

Turn on purge protection to guard against malicious or accidental deletion of the secrets and key vault even after soft-delete is turned on.

Turn on logging for your vault. Also, set up alerts.

Purge protection prevents malicious and accidental deletion of vault objects for up to 90 days. In scenarios, when purge protection isn't a possible option, we recommend backup vault objects, which can't be recreated from other sources like encryption keys generated within the vault.

A multitenant solution is built on an architecture where components are used to serve multiple customers or tenants. Multitenant solutions are often used to support software as a service (SaaS) solutions.


----------------------------------


When storing sensitive and business critical data it must take steps to maximize the security of your vaults and the data stored in them.

You can reduce the exposure of your vaults by specifying which IP addresses have access to them. The virtual network service endpoints for Azure Key Vault allow you to restrict access to a specified virtual network. 
After firewall rules are in effect, users can only read data from Key Vault when their requests originate from allowed virtual networks or internet protocol version 4 address ranges. 
Azure Private Link Service enables you to access Azure Key Vault and Azure hosted customer/partner services over a Private Endpoint in your virtual network.

By default, when you create a new key vault, the Azure Key Vault firewall is disabled. All applications and Azure services can access the key vault and send requests to the key vault. 
This configuration doesn't mean that any user is able to perform operations on your key vault. 
The key vault still restricts access to secrets, keys, and certificates stored in key vault by requiring Microsoft Entra authentication and access policy permissions.

When you enable the Key Vault Firewall, you are given an option to 'Allow Trusted Microsoft Services to bypass this firewall. The trusted services list doesn't cover every single Azure service.

If you're trying to allow an Azure resource such as a virtual machine through key vault, you may not be able to use Static IP addresses, and you may not want to allow all IP addresses for Azure Virtual Machines to access your key vault.
In this case, you should create the resource within a virtual network, and then allow traffic from the specific virtual network and subnet to access your key vault.

Configuring a Virtual Network Service Endpoint for Azure Key Vault allows you to restrict access to the Key Vault based on IP addresses or ranges specified in the virtual network subnet. 
This ensures that only resources within the specified virtual network can access the Key Vault.

Azure Private Link enables access to Azure Key Vault over a private network connection using private IP addresses. 
This ensures that data exchanged between the client and Key Vault remains within the Microsoft Azure backbone network, enhancing security and eliminating exposure over the internet.




# LAB

- Create a key vault using the Azure portal.
- Add an exsiting virtual network to a firewall and virtual network rules.
- Configure a virtual network and subnet to allow access to a key vault.

Create an Azure Key Vault:

Search for Key Vault on search bar and then Create, on the Basics tab of Create a key vault, enter those informations:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/04f74a4d-673c-4ec8-81b0-41bec0bac66f)

Configure Key Vault firewall and virtual network settings:

Under the key vault previously created, select Networking, and then select the Firewalls and virtual networks tab. Select Allow public access from specific virtual networks and IP addresses.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/13da6342-81d3-4f68-b22f-4aa5d300646a)

Under the Virtual networks section, select + Add a virtual network, then select + Add existing virtual networks. In the Add networks template select the vnet and the subnet, then Enable, Add and Apply.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e9b45646-900e-48e2-a9a0-3f81b0d8f14e)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5248ebc9-317b-4260-a579-4723de0ba6ee)

---------------------------

# LAB - Configure Azure Key Vault recovery management with soft delete and purge protection

You can use purge protection to prevent the deletion of your key vault, keys, secrets, and certificates by a malicious insider. Think of this as a recycle bin with a time based lock. You can recover items at any point during the configurable retention period. 
You will not be able to permanently delete or purge a key vault until the retention period elapses. Once the retention period elapses the key vault or key vault object will be purged automatically.

To-do list:
- Verify if soft delete is enabled on a key vault and enable soft delete.
- List, recover, or purge a soft-deleted key vault.
- List, recover or purge soft deleted secrets, keys, and certificates.


Verify if soft delete is enabled on a key vault and enable soft delete:

Select your key vault, Properties blade then, verify if the radio button next to soft-delete is set to Enable purge protection. If soft-delete is not enabled on the key vault, click the radio button to enable soft delete and click Save.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2741a9dd-52a1-4a39-bfdc-ba8fe637329d)


List, recover, or purge a soft-deleted key vault:


Key Vault service > “Manage deleted vaults” > Select your subscription.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1021b137-b75e-46f5-a875-ab1d9cf1661e)

If your key vault has been soft deleted it will appear in the context pane on the right. Once you find the vault you wish to recover or purge, select the checkbox next to it.
Select the recover option at the bottom of the context pane if you would like to recover the key vault.
Select the purge option if you would like to permanently delete the key vault.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/068c8504-2582-4557-afce-366f23393ca5)


List, recover, or purge soft-deleted secrets, keys, and certificates:

Select your key vault. Select the blade corresponding to the secret type you want to manage (keys, secrets, or certificates). Then same steps for key vault.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/761094f8-786e-49b7-9af5-8d781bc245d4)


--------------------
















