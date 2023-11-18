

Resposabilities for IAM
- Create an AWS Acc
- Create Users and Groups
- Access control management
- Authentication and Authorization
- Follow the Principle of Least Privilege
- Audit user access


User wants to use/access AWS services you need IAM Users, so they can access through the management console. Or, the can access using AWS cli (used for automation in the cloud) and SDK (Software Development Kit) Developer will create app to interact with AWS

To manage user use IAM Groups, these gives permissions to a group of users

IAM Roles


Identity Policy - put permissions together to generate a policy, with that policy you can attach to the Users, Group and Roles

Resource Based Policy - To attach Policies to the Resources

Session Policy - When some users need access, but it is temporary connecting to the application and they need a certain permission

Permission Boundary - Least Privilege


# Create an AWS Account

- Best practice is create accounts for every department or organization unit. 
- Consolidate billing in one place for many accounts.

Go to the [aws webpage](aws.amazon.com) 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ece3101e-5e6c-4188-96f8-3163060893b2)

You need to input your email address and put a name of your AWS account.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f56d3e00-06a4-4080-89ae-9f692819c524)

After verify the email, go back to the [aws webpage](aws.amazon.com) and click on Sign in. This is the first access, so select Root user option and provide the email address.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/37a49fea-9867-4399-b654-2c671aac905a)


# IAM User

User to access the services, that user need to be in IAM service and to  have some permissions, the users need to be part of group or role which they have policies configured

### Create IAM User

Seach for IAM in the serach bar or navigation menu.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/a99f0b64-3012-46c7-8650-e6807a2c83eb)

Select User under Access management section and click Create User button

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/20ad9778-c719-4fb7-ae81-530bfb7ca651)

From there specify the username, check the box ***(1) Provide user access to the AWS Management Console***. Create a new password and leave the box checked/enable for ***(2) Users muste create a new password...***

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/b152da8a-c29d-4f99-83ea-34682483593a)

The next step it will be *Set Permissions* Section leave, for now leave as default, then Next and Create user. 

### Test new user create on IAM User

First you need to note the account ID number, which it is the account you create the user. To find the ID number, Sign In with the Root credentials and on the top-right click on the dropbox or you can see the Acc number after @.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9330ab79-4925-46d9-93f7-348d402acf25)

Open (1)Incognito tab in the browser > Sign In > Select (2)IAM user > Fill out the (3)*Account ID* > Next

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ab78d90d-50dd-4512-afac-fd0d1d3c5f59)

Use the credentials user created previously and Sign In

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ab85b4e9-cdc2-47ec-ad6b-d9b8a8731ac7)

It will be prompt a new screen for the user type a new password.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8d25de28-d915-4b39-bea1-ab66a5544188)

Once setup the new password it will prompt the user console.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e4cb2580-11ee-4737-a7a3-f025c0cf280f)


### Use User credentials to run commands using CLI 

First the user created in IAM must have access key created. Using Root Account go to IAM service > Users > Security credentials

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8515e931-8b5a-40e8-8fef-6a8a312a74ef)

Scroll down to the Access keys section > Create access key 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/073cb471-538c-481b-ade8-777c09599442)

In the new pane select (1)CLI and tick the box *(2)I understand the above..."* > Next > Create access key  

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6e1385f3-26af-4820-b7ba-9a25fc7f9dcb)

**ATTENTION!!! ONCE CREATED THE KEY, IF YOU LOSE OR FOORGET YOU WONT BE ABLE TO RETRIEVE IT.**

On Retrieve access keys take note and save in a safe place the Access key and Secret access key

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3ecc010b-9faf-4f34-a4cf-3a3c78ea8024)

Lets access the CLI. In the Root user console, search for Cloudshell service

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6c6a9fce-a51c-491c-8d49-09cafb230163)

First check which identities it is being using with the command:

```
aws sts get-caller-identity
```

This it will give you the user you are logged in this section. In this case it will be the root user. 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/893c377a-231d-4b34-9249-cb9110887380)

To swtich to a new user, if is not created, run the command (aws configure --profile <USER_NAME>):
```
aws configure --profile john
```

It will ask the the ***AWS Access Key ID***. This key should be known and saved in a safe place, check the step [above](#**ATTENTION!!!-ONCE-CREATED-THE-KEY,-IF-YOU-LOSE-OR-FOORGET-YOU-WONT-BE-ABLE-TO-RETRIEVE-IT.**). If is not, 
it will need to create a [new key](####-Use-User-credentials-to-run-commands-using-CLI). The default region and output format just leave as it is.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c197b2cd-d0bb-4f6e-ba08-f64c67ed31cf)

Then running again the command aws sts get-caller-identity it will prompt the new user created.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5e85ab68-5d3b-4173-a055-f5386439873f)

After that this user can communicate with AWS APIs with CLI and SDK.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4228b401-4439-45d0-897f-5810b1f2f601)

These credential can be used for applications to communicate with AWS APIs, then communicate to AWS Services

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5cd51a8e-9b81-4b80-aca7-21fcf33a8e16)


# IAM Group

- IAM Group is used to simnplify user management
- It is assign policy to give specific set of permissions

Under (1)IAM > select (2)User groups > (3)Create Group

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9024abb9-7b6a-4446-8bdb-1f6bd7677cd0)

Then choose a name for a group, select the user that will be part of the group

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e8ca8dbe-8f59-446d-b129-c9baff6b40a7)

Look for a HR Policy (under the search bar(1)) which AWS offers a pre defined policy, select (2)HR Policy. For IT Department usually the policy assign is Administrator Access which provides full access to AWS services.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c1027271-ac0c-4f33-ad71-be74bcb5d64e)

Press the + button(1) next to the policy it is possible to check what the policy gives access. This case gives full access(2) to the buckt called company1-hr. 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6ae78b64-43e7-4ea3-a686-618409941dd3)

Then, click Create group. It will show the group created.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5e733147-8150-4837-92b5-4e797a3cb4b5)

# Create an IAM User and attach Policy directly without user being in a group

When it is creating the IAM User under Step 2 Set Permission(1), choose Attach policies directly(2), search and select the policy desired(3)(4) and on the drop-box Filter by Type choose (5)*AWS Managed - job function*

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f216fe9c-262d-4dcf-a4f7-a863b3be9704)



















