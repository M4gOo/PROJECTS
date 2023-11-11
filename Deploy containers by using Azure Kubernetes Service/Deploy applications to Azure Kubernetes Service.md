


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

Choosing the most suitable storage solution must account for many factors, including performance, availability, recoverability, security, and cost.

When choosing the optimal storage for AKS containerized workloads, you can choose from the following options:
- Application-level access to structured or semi-structured data. For structured or semi-structured data, use a platform managed database, such as Azure SQL, Azure Database by MySQL, Azure Database for PostgreSQL, and Cosmos DB.
- File-level access to data. For shared application data that requires high performance, use either Azure NetApp Files or the premium tier of Azure Files. For shared data that requires moderate performance, use the standard tier of Azure Files.
- Block-level access to data. Use disks for storage for applications that require consistently low latency, high I/O operations per second (IOPS), and high throughput. For the best performance, consider using Azure Premium SSD, Azure Premium SSD v2, or Azure Ultra Disk Storage. Alternatively, apply Azure Blob storage by using BlobFuse virtual file system, or read from and write to blob storage directly.

To implement a persistent storage for pods, you can create volumes. A volume offers a mechanism to store, retrieve, and persist data across pods and throughout the application lifecycle. 
When selecting the underlying storage for AKS volumes, your choices include:
- Azure Disks
- Azure Files
- Azure NetApp Files
- Azure Blobs

### Plan for pod volumes

AKS volume types include:
- emptyDir is used as temporary space for pods. All containers within a pod can access the data on the volume. Data written to this volume type persists only for the lifespan of the pod. Once you delete the pod, the volume is deleted. This volume typically uses the underlying local node disk storage, though it's possible to host it in the node's memory.
- secret is used to inject into pods sensitive data, such as passwords.
- configMap is used to inject into pods key-value pair properties, frequently referencing application configuration settings.
- PersistentVolume (PV) is a block or file storage resource created and managed by the Kubernetes API that has the ability to persist beyond the lifetime of a pod.


### Implement AKS persistent volumes

You can use either Azure Disk or Azure Files resources to implement PersistentVolumes in AKS clusters. The choice between them is typically based on the desired performance characteristics and the ability to provide either shared or exclusive access to the underlying storage. To ensure the availability of persistent volumes, you can precreate PersistentVolume resources. Alternatively, you can rely on the Kubernetes API server to create them dynamically. A pod awaiting deployment, may require storage that is unavailable, AKS can automatically provision an underlying Azure Disk or File resource and attach it to the pod. Dynamic provisioning relies on the StorageClass specification to determine the type of Azure storage to create.


### Create storage classes

The StorageClass is a Kubernetes construct that defines storage characteristics. In AKS, these characteristics map to specific Azure Storage resources.

The StorageClass also defines the reclaimPolicy. When you delete the persistent volume, the reclaimPolicy controls the behavior of the underlying Azure storage resource. That resource can either be deleted or retained for future use. AKS includes several default storage classes, representing a mix of Azure Disks and Azure Files with Standard and Premium performance tiers.

### Configure persistent volume claims

A PersistentVolumeClaim is a request for a particular StorageClass, access mode (which determines whether volume access should be shared or exclusive), and size. As mentioned earlier, the Kubernetes API server can dynamically provision the underlying Azure storage resource if no existing resource can fulfill the claim based on the defined StorageClass.

Once an available storage resource has been assigned to the pod requesting storage, PersistentVolume is bound to a PersistentVolumeClaim.

The following YAML manifest describes a persistent volume claim that uses the managed-premium StorageClass and requests a Disk 5Gi in size:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium-retain
  resources:
    requests:
      storage: 5Gi
```

When you create a pod definition, you also specify:

- the persistent volume claim to request the desired storage.
- the volumeMount for your applications to read and write data.

The following YAML manifest illustrates how that persistent volume claim defined earlier is used to mount a volume on the /mnt/azure directory:

```
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```


# LAB - Configure storage for applications that run on Azure Kubernetes Service


###Create a custom storage class in an AKS cluster

In the Azure portal, in the Bash session of Azure Cloud Shell, use the built-in editor to create a file named premium-storage-class.yaml and copy into it the following YAML manifest. Save the changes to the file and close it to return to the Bash prompt.
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: managed-premium-retain
provisioner: disk.csi.azure.com
parameters:
  skuName: Premium_LRS
reclaimPolicy: Retain
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion:
```

To create the custom storage class, from the Bash session in the Azure Cloud Shell, run the following command:
```
kubectl apply -f premium-storage-class.yaml
```

### Create a persistent volume claim in an AKS cluster

From the Bash session in the Azure Cloud Shell, use the built-in editor to create a file named persistent-volume-claim-5g.yaml and copy into it the following YAML manifest. Save the changes to the file and close it to return to the Bash prompt.
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium-retain
  resources:
    requests:
      storage: 5Gi
```

To create the persistent volume claim, from the Bash session in the Azure Cloud Shell, run the following command:
```
kubectl apply -f persistent-volume-claim-5g.yaml
```


### Deploy a pod with a persistent volume mount in an AKS cluster

From the Bash session in the Azure Cloud Shell, use the built-in editor to create a file named pod-with-storage-mount.yaml and copy into it into the following YAML manifest. Save the changes to the file and close it to return to the Bash prompt.
```
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

To deploy the pod, from the Bash session in the Azure Cloud Shell, run the following command:
```
kubectl apply -f pod-with-storage-mount.yaml
```

### Validate the effect of the volume mount

To verify that the nginx pod was provisioned, from the Bash session in the Azure Cloud Shell, run the following command:
```
kubectl get pods
```

To list the content of the /mnt/azure directory, run the following command:
```
kubectl exec -i nginx -- sh -c "ls /mnt/azure"
```

To create a file named hello containing a single line of text 'Hello world', run the following command:
```
kubectl exec -i nginx -- sh -c "echo 'Hello world' > /mnt/azure/hello"
```

To list the content of the /mnt/azure directory (this time including the newly created hello file, run the following command:
```
kubectl exec -i nginx -- sh -c "echo 'Hello world' > /mnt/azure/hello"
```
To list the content of the /mnt/azure directory (this time including the newly created hello file, run the following command:
```
kubectl exec -i nginx -- sh -c "ls /mnt/azure"
```

To delete the nginx pod, run the following command:
```
kubectl delete pod nginx
```

Now, re-create the nginx pod by running the following command:
```
kubectl apply -f pod-with-storage-mount.yaml
```

Finally, verify that the content of the mount is intact by running the following command:
```
kubectl exec -i nginx -- sh -c "ls /mnt/azure"
```


### Delete the resources provisioned

To list and delete the persistent volume claim, from the Bash session in the Azure Cloud Shell, run the following commands:
```
kubectl get pvc
kubectl delete pvc azure-managed-disk
```

Alternatively, you could run 
```
kubectl delete -f persistent-volume-claim-5g.yaml
```

To list and delete the storage class, run the following commands:
```
kubectl get sc
kubectl delete sc managed-premium-retain
```


# LAB - Deploy an application to Azure Kubernetes Service cluster

the process of creating and updating a deployment in an Azure Kubernetes Service cluster.

### Prepare for creating a deployment to an Azure Kubernetes Service cluster

First, lets create a namespace. From the Azure portal, open a Bash session in the Azure Cloud Shell and run the following commands.
```
kubectl create namespace demo-deployment
kubectl get namespaces
```

Run the following command to add a node to the AKS cluster. Wait until the extra node is fully provisioned before you proceed
```
az aks show --resource-group $AKSRG --name $AKSNAME --query agentPoolProfiles
az aks scale --resource-group $AKSRG --name $AKSNAME --node-count 2 --nodepool-name nodepool1
```

### Create a deployment

In the Azure portal, in the Bash session of Azure Cloud Shell, use the built-in editor to create a file named nginx-deployment.yaml and copy to the following YAML manifest. Save the changes to the file and close it to return to the Bash prompt.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: mcr.microsoft.com/oss/nginx/nginx:1.15.2-alpine
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
            memory: 64Mi
          limits:
            cpu: 500m
            memory: 256Mi
```

To create the deployment, from the Bash session in the Azure Cloud Shell, run the following command:
```
kubectl apply -f nginx-deployment.yaml --namespace demo-deployment
```

To validate the deployment, enumerate deployments, pods, and replica sets by running the following commands:
```
kubectl get deployments --namespace demo-deployment
kubectl get pods --namespace demo-deployment
kubectl get rs --namespace demo-deployment
```

### Update the deployment

To replace the image used by our deployment, from the Bash session in the Azure Cloud Shell, run the following command:
```
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1 --namespace demo-deployment
```

To validate the deployment, enumerate deployments, pods, and replica sets by running the following commands:
```
kubectl get deployments --namespace demo-deployment
kubectl get pods --namespace demo-deployment
kubectl get rs --namespace demo-deployment
```

### Roll back the deployment

roll back the deployment by switching back to the original container image

To display the deployment properties, run the following command:
```
kubectl describe deployment
```

In the output generated by the command, review the replica set information, including the references to the old replica set.

To roll back the deployment, run the following command:
```
kubectl rollouts undo deployment/nginx-deployment --namespace demo-deployment
```

To validate the rollback enumerate deployments, pods, and replica sets by running the following commands:
```
kubectl get deployments --namespace demo-deployment
kubectl get pods --namespace demo-deployment
kubectl get rs --namespace demo-deployment
```

### Delete the resources provisioned

To validate the existence of the resource group you used in this module, from the Bash session in the Azure Cloud Shell, run the following commands:
```
AKSRG='aks-01-RG'
az group show --name $AKSRG
```

To delete the resource group you referenced in the previous step, run the following command:
```
az group delete --name $AKSRG --no-wait --yes
```






















