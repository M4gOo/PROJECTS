
<h2>Requeriments:</h2>

- Filter network traffic with a network security group using the Azure portal
- Create a Log Analytics workspace
- Microsoft Defender for Cloud
- Enable just-in-time access
- Configure and integrate a Log Analytics agent and workspace in Defender for Cloud
- Configure Azure Key Vault networking settings

<h2>What need to be done:</h2>

- Create vnet, subnet, asg, nsg, VMs and make the association necessary
- Create a workspace
- Upgrade your subscription for Microsoft Defender for Cloud and install agents+
- Enable just-in-time access on Virtual Machines
- Collect data from your workloads with the Log Analytics agent
- Configure Key Vault firewall, virtual networks and recovery management with soft delete and purge protection


 
# Filter network traffic with a network security group using the Azure portal

### Create a network security group and security rules.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/61a3f2d6-fc55-4490-8979-a42b3ffbde29)

### Create application security groups.

ASG to enable you to group together servers with similar functions, such as web servers

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f91979cc-4a76-479a-8f1c-3bda76ef1bc5)

### Create a virtual network and associate a network security group to a subnet.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9252e95a-b870-45d3-b91f-c0c7ca3295d4)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9bd5bf62-92f4-4eb0-8ec7-12b8bd274f5c)

### Create security rules for the network security group with the subnet of the virtual network created earlier

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/0fdb9542-43df-4c81-9360-90c7f04b451f)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d5abd907-cf9c-4f8b-a648-d4c5ea8e532d)

### Deploy virtual machines (vm-1 and vm-2) and associate their network interfaces to the application security groups.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c94f34e8-bc41-474e-a246-a6d3829affb4)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2fc66979-cd8c-4277-a39c-d2b7b76aa3cd)

### Associate network interfaces to an application security group

On vm-2 add ASG-mgmt

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6cf49261-2177-4ad0-b091-e2f1d4c5ab02)


# Create a Log Analytic workspace

When you collect logs and data, the information is stored in a workspace. 

### Use the Log Analytics workspaces menu to create a workspace.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a929f463-cb75-4748-b38f-cf2232781fb2)


# Microsoft Defender for Cloud

On the Microsoft Defender for Cloud, Getting started blade, go to the Upgrade tab. Scroll down until the Select subscriptions and workspaces to protect with enhanced security features section is visible. Then click Upgrade

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d3a1c85d-dc97-4a3d-b332-d0b4ffbb101c)

Microsoft Defender for Cloud, Getting started blade, go to the Install agents tab, and scroll down check the box that is associated with the subscription on which agents will be installed, and click Install agents.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/40eaf567-c42d-451f-8a54-9c2a24e83446)


# Enable just-in-time access

If it is showing this page below, need to enable all features

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4576a4d5-7d05-457e-bff1-b974ddb51494)

To enable all features In the Defender for Cloud menu, select Environment settings > Select the subscription or workspace that you want to protect > Select Enable all > Save 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/cad558c9-778c-4b6b-b9d4-f261c9c885cd)


### Enable JIT on your VMs from Microsoft Defender for Cloud

From Defender for Cloud’s menu > Workload protections > Select Just-in-time VM access.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/61083555-7424-47a5-9a45-c179024c5e04)

But the VMs are Unsupported

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4e7a2f13-cd7f-4cfb-ac06-d34219a3f384)

So, it is necessary enable JIT on your VMs from Azure virtual machines. Go to VMs > Configuration > Enable just-in-time

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/62db64e5-ae33-405b-9253-63c4b4b102b3)

Back to Defender for Cloud’s menu > Workload protections > Select Just-in-time VM access. From the Configured tab, right-click on the VM to which you want to add a port, and select edit.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1d0c1a39-556c-4b43-b076-8c74dba63d57)


### Request access to a JIT-enabled VM from the Azure virtual machine’s connect page.

Select the VM to which you want to connect, and open the Connect page. If JIT is enabled, select Request access to pass an access request with the requesting IP, time range, and ports that were configured for that VM.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/aad9a044-1004-4d4f-819b-3cede7807331)


# Configure and integrate a Log Analytics agent and workspace in Defender for Cloud

### Configure integration with the Log Analytics agent.

Defender for Cloud’s menu > Environment settings menu > Select the your subscription

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ebe9c39c-05f7-4b28-af87-5157d1b80bd7)

In the Settings & monitoring Coverage column of the Defender plans, select Settings & monitoring.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2859a963-99a1-48a7-b475-d5211de7c8fa)

From the Configuration options pane, click Edit configuration for Log Analytics agent

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6c8b79ed-5a1c-49c5-a281-6c09c61c5256)

In the Auto-provisioning configuration add the previously created workspace and Under Security events storage select All Events. Then Apply

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f09b21d2-cec8-41c0-a478-4a166df9d8ec)


# Configure Azure Key Vault networking settings


### Use the Azure portal to create an Azure Key Vault.

Search for Key Vault > Create > Fill out with the info below > Create

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8e983fe2-0573-48ee-93a3-a94e89490ddc)

### Configure Key Vault firewall and virtual network settings

Key Vault > select the key vault previously created > Networking > Firewalls and virtual networks tab > Allow public access from specific virtual networks and IP addresses >  Virtual networks section, select + Add a virtual network, then select + Add existing virtual networks.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/b94c1431-cf9c-4475-b0d4-c6471eb12e6a)

In the Add networks template, select the previously created virtual network from the Virtual networks dropdown list and the subnets > click Enable > Add > Apply

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3316bdea-6751-42e7-8c7b-003323eb14ff)


### Verify if soft delete is enabled on a key vault and enable soft delete

Key vault > Properties > Verify if the radio button next to soft-delete is set to Enable purge protection. If soft-delete is not enabled on the key vault, click the radio button to enable soft delete and click Save.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ee00be55-b2d1-4c9e-a4d8-a2f487d390e8)

To manage the keys deleted, soft deleted go the key vault, select the key and manage deleted vaults tab

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/218a8424-98f9-490c-ae1a-1e37f157a7bb)






























 

 
