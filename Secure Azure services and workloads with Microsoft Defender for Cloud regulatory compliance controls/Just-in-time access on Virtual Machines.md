
how to configure and use Microsoft Defender for Cloud's just-in-time (JIT) access to protect your Azure virtual machines (VMs) from unauthorized network access.

You can use Microsoft Defender for Cloud’s just-in-time (JIT) access to protect your Azure virtual machines (VMs) from unauthorized network access. 
Many times firewalls contain allow rules that leave your VMs vulnerable to attack. JIT lets you allow access to your VMs only when the access is needed, on the ports needed, and for the period of time needed


To-do list:

- Enable JIT on your VMs from the Azure portal.
- Request access to a VM that has JIT enabled from the Azure portal.


<h2>Enable JIT on your VMs from Microsoft Defender for Cloud<h2/>

- From Defender for Cloud’s menu, open Workload protections.
- Select Just-in-time VM access.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9d0b3ee6-729e-4e09-9b56-ff2f572c6386)

- In the Not configured virtual machines, mark the VMs to protect with JIT and select Enable JIT on VMs.
  -  SS - SSH (Secure Shell)
  -  3389 - RDP (Remote Desktop Protocol)
  -  5989 - WinRM (Windows Remote Management)
  -  5986 - WinRM (Windows Remote Management)
    - To customize the JIT access:
    - Select Add.
    - Select one of the ports in the list to edit it or enter other ports. For each port, you can set the:
         - Protocol 
         - Allowed source IPs  
         - Maximum request time
    - Selet OK.


<h3>Request access to a JIT-enabled VM from Microsoft Defender for Cloud<h3/>

- From Defender for Cloud’s menu, open Workload protections.
- Select Just-in-time VM access, select the Configured tab. 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5ba33900-3bb3-42b2-834d-8cc3def356c7)

- Select the VMs you want to access.
  - The icon in the Connection Details column indicates whether JIT is enabled on the network security group or firewall. If it’s enabled on both, only the firewall icon appears.


<h3>Enable JIT on your VMs from Azure virtual machines<h3/>

- From the Azure portal, search for and select Virtual machines.
- Select the virtual machine you want to protect with JIT.
- In the menu, select Configuration.
- Under Just-in-time access, select Enable just-in-time.
- By default, just-in-time access for the VM uses these settings:

```
Windows machines

RDP port: 3389
Maximum allowed access: Three hours
Allowed source IP addresses: Any
Linux machines

SSH port: 22
Maximum allowed access: Three hours
Allowed source IP addresses: Any

```

- By default, just-in-time access for the VM uses these settings:
  - From Defender for Cloud’s menu, select Just-in-time VM access.
  - From the Configured tab, right-click on the VM to which you want to add a port, and select edit.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1cbfd0a4-24a4-456e-888e-608790b9aced)

- Under JIT VM access configuration, you can either edit the existing settings of an already protected port or add a new custom port.
- When you’ve finished editing the ports, select Save.


<h3>Request access to a JIT-enabled VM from the Azure virtual machine’s connect page.<h3/>

- In the Azure portal, open the virtual machines pages.
- Select the VM to which you want to connect, and open the Connect page.
  - Azure checks to see if JIT is enabled on that VM.
    - If JIT isn’t enabled for the VM, you’re prompted to enable it.
    - If JIT is enabled, select Request access to pass an access request with the requesting IP, time range, and ports that were configured for that VM.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/942655bd-c9e1-4196-bb8b-ce4572ecfe0c)




















