

Azure Policy enforces policies and safeguards on your Kubernetes clusters at scale. Azure Policy Ensures that your cluster is secure, compliant, and consistent across your organization.
Azure Policy is an add-on that enables centralized, consistent enforcement of policies and safeguards on Kubernetes clusters at scale.
For example, you can use Azure Policy to enforce policies such as disallowing root privileges or requiring specific labels on pods. 
Azure Policy applies at scale enforcements and safeguards on Kubernetes clusters in a centralized, consistent manner to ensure secure and stable application hosting platforms

Azure Policy extends Gatekeeper v3, an admission controller webhook for Open Policy Agent (OPA), to apply at-scale enforcements and safeguards on your clusters in a centralized, consistent manner.

https://open-policy-agent.github.io/gatekeeper/website/docs/howto/#constraint-templates

To enable and use Azure Policy with your Kubernetes cluster, take the following actions:
- Configure your Kubernetes cluster and install the Azure Kubernetes Service add-on.
- Understand the Azure Policy language for Azure Kubernetes Service Kubernetes.
- Assign a definition to your Azure Kubernetes Service cluster.
- Wait for validation.

Recommendations
The following are general recommendations for using the Azure Policy Add-on:

- The Azure Policy Add-on requires three Gatekeeper components to run: One audit pod and two webhook pod replicas. These components consume more resources as the count of Kubernetes resources and policy assignments increases in the cluster, 
which requires audit and enforcement operations.

  - For fewer than 500 pods in a single cluster with a max of 20 constraints: two vCPUs and 350-MB memory per component.
  - For more than 500 pods in a single cluster with a max of 40 constraints: three vCPUs and 600-MB memory per component.


# LAB - Enable Azure Policy add on for Azure Kubernetes Service

Select K8s cluster, then Policies and select Enable add-on

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9cedd856-06c9-4e91-96bb-a61fa0f5eefb)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/cfd5087c-89d6-4e12-a10e-24f754cec6b9)

On the Azure portal menu, search for Subscriptions. Select your Azure subscription. Select Resource providers, and search for Microsoft.PolicyInsights. Verify that the Microsoft.PolicyInsights Resource provider is registered.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f64823c5-696f-49ef-aa4c-38ed544f766e)


# LAB - Assign a policy definition to an Azure Kubernetes cluster

To assign a policy definition to your Kubernetes cluster, you must be assigned the appropriate Azure role-based access control (Azure RBAC) policy assignment operations. The Azure built-in roles Resource Policy Contributor and Owner have these operations.
If using a custom policy definition, search for it by name or the category that you created it with.

Search for Policy, then Definitions

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/716b0c30-699d-4973-9efb-d43797b2772d)

Category dropdown list box, use Select all to clear the filter and then select Kubernetes.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5d2baa94-1e22-46d5-94fa-d63c6e206399)

Select the policy definition, then select the Assign button.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/07a70214-2c87-4f7f-8239-a2456b3c6f1a)

Set the Scope to the management group, subscription, or resource group of the Kubernetes cluster where the policy assignment applies.
The Scope must include the cluster resource when assigning the Azure Policy for Kubernetes definition.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/b3faf7f6-977c-4b23-8a7f-e04007598f67)

Give the policy assignment a Name and Description that you can use to identify it easily.

Set the Policy enforcement to one of the values.
- Enabled - Enforce the policy on the cluster. Kubernetes admission requests with violations are denied.
- Disabled - Don't enforce the policy on the cluster. Kubernetes admission requests with violations aren't denied. Compliance assessment results are still available. The Disabled option is helpful for testing the policy definition as admission requests with violations aren't denied.


Then select Next, Set parameter values, Create

Select Overview in the left pane and then search for and select Policy. Under Name, view the compliance for the Policy definition

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ea2d3e4a-79b6-4b0e-ba44-694f427bc02c)



# LAB - Create a custom namespace

on K8s cluster select namespace, select create then Namespace. In create a namespace, enter a name for the namespace, and select Create.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/99e73e18-445a-480c-ba7e-257c19689ca5)

Select the newspace, select YAML. Select Review + save to save YAML updates to the namespace.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/079351b4-d9ae-4d1c-a845-cb03c0fa35dc)


















