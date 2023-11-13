

Configuring a Security information and event management (SIEM) for Security Operations using Microsoft Sentinel, involves provisioning a Log Analytics workspace and configuring the Microsoft Sentinel options.


<h2>Requirements:</h2>
  
- [Configure SIEM operations using Microsoft Sentinel](#Configure-SIEM-operations-using-Microsoft-Sentinel)
- [Install Microsoft Sentinel Content Hub solutions and data connectors](#Install-Microsoft-Sentinel-Content-Hub-solutions-and-data-connectors)
- [Configure a data connector Data Collection Rule](#Configure-a-data-connector-Data-Collection-Rule)
- [Perform a simulated attack to validate the Analytic and Automation rules](#Perform-a-simulated-attack-to-validate-the-Analytic-and-Automation-rules)


<h2>What need to be done:</h2>
  
- Create an Azure Log Analytics workspace and deploy Microsoft Sentinel to the workspace
- Configure Microsoft Sentinel to ingest data by using Microsoft Sentinel solutions
- Configure a data connector Data Collection Rule (DCR), detect threats with near-real-time (NRT) analytic rules, and configure automation in Microsoft Sentinel
- Perform a privilege escalation on VM1 to validate that the Analytic and Automation rules create an incident and assign it to the owner


# Configure SIEM operations using Microsoft Sentinel


### Create a Log Analytics workspace

Search for Microsoft Sentinel > Create > Create new workspace > Fill out with the informations (Resource Group, name and Region)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9eb351b0-3f16-467d-a5d6-32b1cd9d3648)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/20012306-feed-4eec-a4e7-c133816f0144)


### Deploy Microsoft Sentinel to a workspace

Once the workspace is created refresh the page and then add to Sentinel selecting the workspace and Select Add.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4e192e24-c6d8-41f0-a594-31ae6b589f4d)


### Assign a Microsoft Sentinel role to a user

Go to the Resource group RG2 > Access control (IAM) >  Add and Add role assignment

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9796f2de-f6ea-4ef5-ba7a-8a25c65f69cb)

In the search bar, search for and select the Microsoft Sentinel Contributor role > Next

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/417612d3-fc18-4a0a-aa07-b925a651cccd)

Select the option User, group, or service principal > + Select members > Search for the Operator1 (operator1-XXXXXXXXX@LODSPRODMCA.onmicrosoft.com) > Select the user icon >  Select > Select “Review + assign” (x2)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8936b40d-0777-4578-b9bd-55b74b86001d)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/811875a5-504d-4db8-b162-97d9222b9021)

To confirm tthe assigned 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8bb37c61-133e-4082-a352-31a37bda5720)


### Configure data retention

Log Analytics workspace > Usage and estimated costs > Data retention > Set up data retention period to any days desired > OK.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c4824318-0ab2-49a7-b012-f05b6405700e)


# Install Microsoft Sentinel Content Hub solutions and data connectors

### Deploy a Microsoft Sentinel Content Hub solution

In Microsoft Sentinel > Content Hub > Windows Security Events > Click View details

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/d5568e8b-c3c0-494a-a0b5-f756c5b5db05)

Select Windows Security Events plan > Create

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6da8dc4e-4b62-4f78-861b-a19d4ed600a0)

RG2 resource group that includes the Microsoft Sentinel workspace and select the workspace. Review+Create

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/39d193af-94db-40b9-8137-5543d95030e1)

Repeat these steps for the Azure Activity and the Microsoft Defender for Cloud solutions.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6f58bd49-f8b4-40c1-b894-4a39c0fb1582)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/cef68f37-6ec6-4a4f-bce8-ee76d40375e2)


### Set up the data connector for Azure Activity

Configure the data connector for Azure Activity to apply all new and existing resources in the subscription

Microsoft Sentinel > Content Hub > filter Status for Installed solutions > Select the Azure Activity solution > Manage

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5308c100-ffd5-4a01-b5fa-e423385a7623)

Azure Activity Data connector > Open connector page

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1cc6e717-9b6a-43cf-a7e5-cfe71b4851d4)

scroll down and select Launch Azure Policy Assignment Wizard>.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a600a499-d74b-4bf0-b20e-f63f4253e8bf)

Basics tab > select (…) > under Scope select your subscription

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/973b46ad-7ff3-421d-bd65-c71f1438b54d)

Parameters tab > choose workspace from the Primary Log Analytics workspace 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/99533814-0755-43c3-a901-6ed3989e9de7)

Remediation tab > select the checkbox (Create a remediation task). Then Review+Create

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/00fbe654-0534-425b-bc53-956c2d6a797c)


### Set up the for Defender for Cloud data connector

Configure the data connector for Microsoft Defender for Cloud and ensure that that only incident management is configured.

Microsoft Sentinel > Content Hub > Select the Microsoft Defender for Cloud solution > Manage > Select the Microsoft Defender for Cloud Data connector > Open connector page > 
In the Configuration area under the Instructions tab, scroll down to your subscription and move the slider in the Status column to Connected (Make sure Bi-directional sync is Enabled)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6f1cd9d1-e8fc-4b10-bd5c-e5d9188157b9)


### Create an analytics rule

Create an analytic rule based on the Suspicious number of resource creation or deployment activities template. The rule should run every hour and only lookup data for that last hour


Microsoft Sentinel > Analytics (Configuration menu section) > Rule templates tab search and select for Suspicious number of resource creation or deployment activities > Create rule

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/65e963a5-dcd9-4f39-bf14-c966053750cf)

This Section only need to configure is in the Set rule logic > Configure Query scheduling > Review+create (Save)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4de09d25-a35f-4583-b640-091ef0ed417b)


### Ensure that the Azure Activity workbook is available in My workbooks

In Microsoft Sentinel > Content Hub > Azure Activity solution > Manage

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/cdf10c28-9302-4777-992a-82e43f83d3ef)

Select the Azure Activity workbook checkbox, and then select Configuration.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c0aecff6-7676-4198-a1b8-5f8416e023f8)

Select the Azure Activity workbook and select Save. Choose the Azure Region for your Microsoft Sentinel workspace.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/61dbfeb2-1739-494a-8254-b11c7a4e5002)


# Configure a data connector Data Collection Rule

We need to configure Microsoft Sentinel to receive security events from virtual machines that run Windows.

### Configure Data Collection rules (DCRs) in Microsoft Sentinel

In Microsoft Sentinel > Data connectors > Windows Security Events via AMA > Open connector page

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6e6701f0-b1f8-4b69-ba23-53e7b8559919)

In the Configuration area under the Instructions tab > select +Create data collection rule > On the Basics tab enter a Rule Name

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/361eedd7-c67a-4903-a7ed-20abbf803241)

On the Resources tab select VM1

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ce0461b1-2770-4f4d-b13c-8442659dc7f3)

On the Collect tab leave the default of All Security Events, then Create


### Create a near real-time (NRT) query detection threats

In Microsoft Sentinel > Analytics > +Create NRT query rule
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ae9ec70d-7e3c-4803-afb4-9a99974364f9)

Enter a Name for the rule, and select Privilege Escalation from Tactics and techniques

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/07b3acd1-8fbc-4de5-94fc-bd3147a26f5d)

Set rule logic >  Enter the KQL query into the Rule queryform. Then, Review + Create and Save.
```
SecurityEvent 
| where EventID == 4732
| where TargetAccount == "Builtin\\Administrators"
```

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/97bbeaa9-c164-4b8b-83c3-0a87165c64ee)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1a0727a3-e6af-48f5-83fb-19cfd2a22f6f)


### Configure automation in Microsoft Sentinel


Microsoft Sentinel > Automation > + Create > Automation rule

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/0ebe8c36-a7d9-46c1-b3fc-dc81a01a55c2)

Enter an Automation rule name > Assign owner from Actions > Apply

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/05d8831e-edb9-4ccd-b58d-440b327a386c)


# Perform a simulated attack to validate the Analytic and Automation rules


###  Perform a simulated Privilege Escalation attack

select the VM1 virtual machine > Run command (operations section) > RunPowerShellScript

Copy the commands below to simulate the creation of an Admin account into the PowerShell Script form and select Run. It is possible to rerun the commands by changing the username.

```
net user theusernametoadd /add
net user theusernametoadd ThePassword1!
net localgroup administrators theusernametoadd /add
```

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e8af6023-d5f3-4fef-91e2-d5ef73aadec0)

In the Output window you should see The command completed successfully three times

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c48120bc-5d39-4fa6-b60b-3e265b7558df)


### Verify an incident is created from the simulated attack

Microsoft Sentinel > Incidents (Threat management menu section) > Select the Incident and the detail pane opens
- The Owner assignment should be <owner_name> (created from the Automation rule)
- Tactics and techniques should be Privilege Escalation (from the NRT rule)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4d913a99-bc4d-4946-a894-25c858a8c8ce)

Select View full details to see all the Incident management capabilities and Incident actions

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/0092a96f-89fd-498e-b3b1-f84ac8453884)









