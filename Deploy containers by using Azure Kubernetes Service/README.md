

# PROJECT - Deploy applications to Azure Kubernetes Service

- [Provisioning Azure Container Registry (ACR) and Azure Kubernetes Service (AKS)](#Provision-Azure-Container-Registry-(ACR)-and-Azure-Kubernetes-Service-(AKS))
- [Building a Linux and Windows container images and store them in Azure Container Registry](#Build-Linux-and-Windows-container-images-to-store-in-the-registry)
- [Deploying container images to Azure Kubernetes Service](#Deploy-container-images-to-Azure-Container-Registry)


# Provision Azure Container Registry (ACR) and Azure Kubernetes Service (AKS)

###  Create an Azure Container registry

Search for Container registries > Create > specify the following settings:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2929dcba-9d7c-44c0-ab2c-8d848cdff30e)


### Create an Azure virtual network and an AKS cluster

Search for Virtual networks > specify the following settings (use the same region as ACR):

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f4dce6de-3fe2-4a01-824d-6ce415cca4a6)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fdf2e1e3-0068-485c-9ea0-2de83e923923)

add a subnet as well

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7af98423-25ac-4926-9944-e27ce82eef8a)



Search for Kubernetes services > + Create > in the dropdown list, select Create a Kubernetes cluster > specify the following settings:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/70c8748f-46fd-4517-968c-8efcdb9ddbcc)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/cbadde55-bed3-49cf-87ed-59da5714805e)

On the Node pools > +add >  specify the following settings:

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/369a4f12-d888-4b33-9ac5-dea7c307d6dd)

Integration tab >  Container registry dropdown list, select the entry representing the Azure Container registry created previously. Then Review+Create.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/2cde6520-08f9-4c6f-92b5-68e86bc188bf)


# Build Linux and Windows container images to store in the registry


### Build a Linux container image and store them in ACR

use a ACR task to build a Linux container image and automatically push it into the ACR

select the Cloud Shell icon > Bash > Create storage > Ensure that Bash appears in the drop-down menu in the upper-left corner of the Cloud Shell pane

create a directory that will host the Dockerfile for the Linux image and change to it from the current directory by running the following commands:
```
mkdir ~/image-l01
cd ~/image-l01
```

use the built-in editor to create a file named server.js in the image-l01 directory and copy into it the following content:
```
const http = require('http')
const port = 80
const server = http.createServer((request, response) => {
  response.writeHead(200, {'Content-Type': 'text/plain'})
  response.write('Hello World from Node\n')
  response.end('Version: ' + process.env.NODE_VERSION + '\n')
})
server.listen(port)
console.log(`Server running at http://localhost: ${port}`)
```

use the built-in editor again to create a file named package.json in the image-l01 directory and copy into it the following content:
```
{
  "name": "helloworld",
  "version": "1.0.0",
  "description": "Sample app for ACR Build",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js"
  },
  "license": "MIT"
}
```

use the built-in editor again to create a file named Dockerfile in the image-l01 directory and copy into it the following content:
```
FROM node:20.2-alpine
COPY . /src
RUN cd /src && npm install
EXPOSE 80
CMD ["node", "/src/server.js"]
```

Use the name of your Azure Container registry you created earlier and store it in a variable named $ACRNAME by running the following commands:
```
ACR_RGNAME='acr-01-RG'
ACR_NAME=$(az acr list --resource-group $ACR_RGNAME --query "[].name" --output tsv)
```

Create a Docker image based on the Dockerfile stored in the current directory and push it automatically to the Azure Container registry which name is stored in the $ACRNAME variable by running the following command:
```
az acr build --registry $ACR_NAME --image hellofromnode:v1.0 .
```

You can check teh repo 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5924eea6-2b7f-49eb-85db-e11cd17fd808)



### Build a Windows container image and store them in ACR

From the Bash session within Cloud Shell, create a directory that will host the Dockerfile for the Windows image and change to it from the current directory by running the following commands:
```
mkdir ~/image-w01
cd ~/image-w01
```

Clone a public GitHub repo hosting the files you’ll use to build the Windows image and switch the current directory to the cloned repo by running the following commands:
```
git clone https://github.com/Azure-Samples/dotnetcore-docs-hello-world.git
cd ~/image-w01/dotnetcore-docs-hello-world
```

create a Docker image based on the Dockerfile stored in the current directory and push it automatically to the Azure Container registry which name is stored in the $ACRNAME variable by running the following command:
```
az acr build --registry $ACR_NAME --image hellofromdotnet:v1.0 --platform windows --file Dockerfile.windows .
```


# Deploy container images to Azure Container Registry

### Create custom AKS namespaces

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/029309e1-1885-4bce-91bf-20063f16b743)

### Create a Kubernetes manifest for deploying the Linux image

Create a Kubernetes manifests for deploying the Linux image to the Linux node pool

Cloud Shell icon >  Bash > create a directory that will host the deployment manifest for provisioning pods based on the Linux image and change to it from the current directory by running the following commands:
```
mkdir ~/aks-l01
cd ~/aks-l01
```

Use the built-in editor to create a file named aks-deployment-l01.yaml in the aks-l01 directory and copy into it the following content:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hellofromnode-deployment
  labels:
    environment: dev
    app: hellofromnode
spec:
  replicas: 1
  template:
    metadata:
      name: hellofromnode
      labels:
        app: hellofromnode
    spec:
      nodeSelector:
        "kubernetes.io/os": linux
      containers:
      - name: hellofromnode
        image: ACR_NAME.azurecr.io/hellofromnode:v1.0
        resources:
          limits:
            cpu: 1
            memory: 800M
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: hellofromnode
---
apiVersion: v1
kind: Service
metadata:
  name: hellofromnode-service
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: hellofromnode
```

Replace the ACR_NAME placeholder in the file aks-deployment-l01.yaml, by running the following commands:
```
ACR_RGNAME='acr-01-RG'
ACR_NAME=$(az acr list --resource-group $ACR_RGNAME --query "[].name" --output tsv)
sed -i "s/ACR_NAME/$ACR_NAME/" ./aks-deployment-l01.yaml
```

### Create a Kubernetes manifest for deploying the Windows image

Create a Kubernetes manifests for deploying the Windows image to the Windows node pool

Create a directory that will host the deployment manifest for provisioning pods based on the Windows image and change to it from the current directory by running the following commands:
```
mkdir ~/aks-w01
cd ~/aks-w01
```

Use the built-in editor to create a file named aks-deployment-w01.yaml in the aks-w01 directory and copy into it the following content:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hellofromdotnet-deployment
  labels:
    environment: dev
    app: hellofromdotnet
spec:
  replicas: 1
  template:
    metadata:
      name: hellofromdotnet
      labels:
        app: hellofromdotnet
    spec:
      nodeSelector:
        "kubernetes.io/os": windows
      containers:
      - name: hellofromdotnet
        image: ACR_NAME.azurecr.io/hellofromdotnet:v1.0
        resources:
          limits:
            cpu: 1
            memory: 800M
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: hellofromdotnet
---
apiVersion: v1
kind: Service
metadata:
  name: hellofromdotnet-service
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: hellofromdotnet
```

Replace the ACR_NAME placeholder in the file aks-deployment-l01.yaml by running the following command:
```
sed -i "s/ACR_NAME/$ACR_NAME/" ./aks-deployment-w01.yaml
```

### Perform AKS deployments by using YAML manifest files

Deploy both container images to their respective namespaces and node pools in the target AKS cluster.

Connect to the AKS cluster, by running the following commands:
```
AKSRG='aks-01-RG'
AKSNAME='aks-01'
az aks get-credentials --resource-group $AKSRG --name $AKSNAME
```

verify that the connection was successful, by running the following command:
```
kubectl get nodes
```

Create the first deployment defined in the corresponding YAML manifest file in the dev-node namespace by running the following commands:
```
cd ~/aks-l01
kubectl apply -f aks-deployment-l01.yaml -n=dev-node
```

Create the second deployment defined in the corresponding YAML manifest file by running the following command:
```
cd ~/aks-w01
kubectl apply -f aks-deployment-w01.yaml -n=dev-dotnet
```


### Review the deployment and deprovision all resources

Display the status of both deployments by running the following commands:
```
kubectl get deployments -n=dev-node
kubectl get deployments -n=dev-dotnet
```

Display the status of the two services which were included in the manifest files by running the following commands:
```
kubectl get services -n=dev-node
kubectl get services -n=dev-dotnet
```

Verify that the listing for each service includes a value in the EXTERNAL-IP column.

Use a web browser to navigate to the IP addresses you identified in the previous step and verify that the resulting web pages display the Hello World from Node and Hello World from .Net 7 messages, respectively.

























