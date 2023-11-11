


# Configure Azure Kubernetes pods using Azure Policy

Azure Policy for AKS uses the admission controller functionality implemented by Gatekeeper v3, to apply at-scale enforcements and safeguards in a centralized, consistent manner. 
Azure Policy makes it possible to manage and report on the compliance state of your AKS environment.

>You can implement security policies for individual AKS clusters (without relying on Azure Policy) by using Pod Security Admission. 
Pod Security Admission is a built-in policy solution suitable for individual cluster deployments. In enterprise scenarios, you should consider using Azure Policy-based policies instead.


# LAB - Apply Azure Kubernetes Service pod settings using Azure Policy


### Deploy an AKS cluster

In the Azure portal, select the Azure Cloud Shell icon, select Bash

To create a resource group to host the AKS cluster, in the Bash session in the Azure Cloud Shell, run the following commands. 
```
AKSRG='aks-01-RG'
LOCATION='eastus'
az group create --name $AKSRG --location $LOCATION
```

To create an AKS cluster, run the following commands
```
AKSNAME='aks-01'
az aks create --resource-group $AKSRG --name $AKSNAME --enable-managed-identity --node-count 1 --generate-ssh-keys
```

Once the cluster provisioning completes, to connect to the AKS cluster, run the following command:
```
az aks get-credentials --resource-group $AKSRG --name $AKSNAME
```

To verify that the connection was successful, run the following command. The output of the command should include the listing of the AKS nodes.
```
kubectl get nodes
```

### Install the Azure Policy add-on for AKS

To install the Azure Policy add-on, you need to ensure that the Microsoft.PolicyInsights resource provider is registered in your subscription. From the Bash session in the Azure Cloud Shell in the Azure portal, run the following commands to verify.
Review the output and, if necessary, wait until the provider status changes to registered (you can run the second command to display the updated status).
```
az provider register --namespace Microsoft.PolicyInsights
az provider show --namespace Microsoft.PolicyInsights --output table
```

To install the add-on, run the following commands. It's possible to enable the add-on during cluster deployment.
```
AKSRG='aks-01-RG'
AKSNAME='aks-01'
az aks enable-addons --addons azure-policy --name $AKSNAME --resource-group $AKSRG
```

To validate that the add-on installation was successful and that the azure-policy and gatekeeper pods are operational, run the following commands:
```
kubectl get pods --namespace kube-system
kubectl get pods --namespace gatekeeper-system
```

To verify that, at this point, no Azure Policy constraints are applied to the target cluster, run the following command. The command should generate output No resources found.
```
kubectl get constrainttemplates
```

### Assign an Azure Policy initiative to an AKS cluster

- In the Azure portal search for and select Policy.
- In the left pane of the Azure Policy page, select Definitions.
- From the Category dropdown list box, use Select all to clear the filter and then select Kubernetes.
- In the Definition type dropdown list, select Initiative.
- In the list of the filtered policies, select the policy initiative named Kubernetes cluster pod security baseline standards for Linux-based workloads, and then select the Assign initiative.
- On the Basics tab of the Assign initiative page, set the Scope to your Azure subscription and the resource group named aks-01-RG, which hosts the newly deployed AKS cluster.
- Ensure that the Policy enforcement is set to Enabled, and then select Next.
- On the Basics tab of the Assign initiative page, select Next.
- On the Advanced tab, select Next.
- On the Parameters tab, remove the Only show parameters that need input or review parameter. Next, in the Effect drop-down list, select Deny and then select Review + create.
- On the Review + create tab, select Create.
- Wait until the assignment takes effect (about 20 minutes). In the Azure portal, navigate to the Azure Policy page, select Compliance, and check if it displays the compliance status for the newly created policy assignment. Alternatively, you can rerun the kubectl get constraint templates command.


### Validate the effect of Azure Policy

in the Bash session of Azure Cloud Shell, use the built-in editor to create a file named nginx-privileged.yaml and copy it into the YAML manifest.
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
      securityContext:
        privileged: true
```

Save the changes to the file and close it to return to the Bash prompt. Attempt deploying a pod based on the YAML manifest by running the following command:
```
kubectl apply -f nginx-privileged.yaml
```

Verify that the deployment fails with an error message that resembles the following one.
```
Error from server (Forbidden): error when creating "nginx-privileged.yaml": admission webhook "validation.gatekeeper.sh" denied the request: [azurepolicy-k8sazurev2noprivilege-a759592cb6adc510dcfa] Privileged container isn't allowed: nginx-privileged, securityContext: {"privileged": true}
```


# Configure storage for applications running on Azure Kubernetes Service












