
#
### AWS IAM Identity Center   


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
### IAM user to be an ADMIN    -  AWS Identity and Access Management (IAM)

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

Providing users credentials (temporary) results in enhanced security for your AWS account, because they are generated each time the user signs in.
Users are automatically granted access to the the AWS Billing and Cost Management console.











