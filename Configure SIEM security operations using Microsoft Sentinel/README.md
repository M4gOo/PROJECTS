

Configuring a Security information and event management (SIEM) for Security Operations using Microsoft Sentinel, involves provisioning a Log Analytics workspace and configuring the Microsoft Sentinel options.


# Requirements:
[Configure SIEM operations using Microsoft Sentinel]
[Install Microsoft Sentinel Content Hub solutions and data connectors]


# What need to be done:
Create an Azure Log Analytics workspace
Deploy Microsoft Sentinel to the workspace
Configure Microsoft Sentinel to ingest data by using Microsoft Sentinel solutions



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


















