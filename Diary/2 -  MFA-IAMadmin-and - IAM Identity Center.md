
#
### AWS IAM Identity Center   

Bottom line:
* IAM Identity Center you can manage all AWS accounts within an AWS Organization, is ideal for managing multiple AWS accounts and also other cloud applications. 
* IAM Identity Center allows you to create, manage, and delete users and groups centrally. This makes it easier to keep track of your users and their permissions.

This service provides temporary credentials that are granted each time a user signs in for a session. 

It can integrate with any existing identity providers you might already have, like Microsoft Active Directory - users can use the same sign on for AWS as they use for other services in your organization. 

If you don't have another identity provider, you can create users in IAM Identity Center. 
This is the recommended way to create additional users for your AWS account.

IAM Identity Center is where you connect your existing directory or use the built-in Identity Center directory to manage user access to AWS accounts and cloud applications. You can more easily discover who can access what.

With application assignments, you can give users in Identity Center access to Identity Center enabled applications, such as Amazon Sagemaker Studio, and SAML 2.0 applications, 
such as Salesforce and Microsoft 365. Your users can access these applications in a single place, without the need for you to set up separate federation.

* SAML (Security Assertion Markup Language) - it allows a user to authenticate in a system and gain access to another system by providing proof of their authentication.

When you enable IAM Identity Center you also need to enable AWS Organizations.

# 
### AWS Organizations  - https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html

AWS Organizations lets you organize multiple AWS accounts so that you can have separate AWS accounts for different use cases. 

AWS Organizations is a feature of your AWS account offered at no additional charge.

* Enables single payer and centralized cost tracking
* Lets you create and invite other AWS accounts
* Allows you to apply policy-based controls
* Helps you simplify organization-wide management of AWS services
  
  
#
### IAM user to be an ADMIN   

Securely manage access to AWS services and resources per AWS account.

This service provides access control policies and manages long-term users like the root user. 

If you create users in IAM, those users have long-term access credentials. 

As a security best practice, it is recommended that you minimize the use of long-term credentials in AWS.



![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4c3755a2-965a-4d4c-bb9e-b0980f6a73d6)




#### ADD SECURITY - MFA


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/124c1917-f128-4b7b-8156-715aab901eaf)


Just follow instructions and it will be assigned

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/b217dbae-78d7-4a18-a86a-3ccfa946f2dc)



####  IAM Identity Center

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/0e939e6f-548b-417d-b7f5-5504e91ae3e2)


#

* create user
  
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/703a8910-26d1-454f-8471-dc2b92e979f3)

just press the button add users


* email must be unique. So save time and creating email I used the + in my gmail account
* optional information can be added later.
 
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/0132f964-fe89-4c76-b890-44fb9dd25da3)


If you can’t find the email, sign in as the root user and reset the Identity Center user’s password


* create a group

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a33471ae-08e1-483a-94c3-c8199c138b0b)


#

* create permission set for admin 
  
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/146b0792-4b19-4a5f-9c0f-7663230c1bce)


#

Your new user exists but does not have access to any resources, services, or applications, so the user can't replace your root user for daily administrative tasks yet 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/0e60dab4-0b31-48e7-be38-876e1b098133)


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f5222c26-c71c-4316-a891-1d08ed886a33)


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/44b5d8ab-3a3f-4980-956a-c80242d5aaac)


The next to steps leave as by default configuration.


#

Assign users/groups with those admin permissions


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c0cbb333-ec5f-4595-9d42-716b4f1276a4)


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1f431f9e-f393-4d7a-9ae8-07e35168dd32)


* select the users(s)/group(s) you want to assign the permissions


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5a5ceaa4-db1e-414d-8919-41dc7c14a7b0)


![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3e033c17-a8e8-4837-ac0d-4c0b0c72f8d4)




