

Create a private registry service for building, storing, and managing container images and related artifacts
Azure Container Registry is a private registry service for building, storing, and managing container images and related artifacts. 

- [Create a container registry](#Create-a-container-registry)
- [Sign in to the container registry](#Sign-in-to-the-container-registry)
- [Push an image to the registry](#Push-an-image-to-the-registry)
- [View container images](#View-container-images)
- [Run an image from the registry](#Run-an-image-from-the-registry)


### Create a container registry

Create a Resource > Create container registry 

The registry name must be unique within Azure. Then Create

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/15af9deb-abab-4880-bbac-8ce3ffb59859)

Go to resource > Overview > Check the registry name and the value of the Login server, which is a fully qualified name ending with azurecr.io in the Azure cloud. This is used to push and pull images with Docker.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/655f0cf2-4595-4f9c-a695-bdd162864ccc)


### Sign in to the container registry

Before pushing and pulling container images, you must sign-in to the registry instance.

Sign into the Azure CLI on your local machine, then run the az sign-in command. Specify only the registry resource name when signing in with the Azure CLI.

```
az login
```

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/002277c0-110a-461c-990e-d4f305637001)

Sign-in to the registry using the resource name.

```
az acr login --name <registry-name>
az acr login --name newregistryapl.azurecr.io
```


### Push an image to the registry

Docker commands are used to push a container image into the registry, and finally pull and run the image from your registry.
You must also have Docker installed locally and running. 

To push an image to an Azure Container registry, you must first have an image. For example, pull the hello-world image from Microsoft Container Registry.

```
docker pull mcr.microsoft.com/hello-world
```

**Before** you can push an image to your registry, you *must tag it with the fully qualified name of your registry login server*.

Tag the image using the docker tag command. Replace <login-server> with the login server name of your ACR instance.

```
docker tag mcr.microsoft.com/hello-world <login-server>/hello-world:v1
docker tag mcr.microsoft.com/hello-world newregistryapl.azurecr.io/hello-world:v1
```


### Use docker push to push the image to the registry instance. 

```
docker push <login-server>/hello-world:v1
docker push newregistryapl.azurecr.io/hello-world:v1
```


### View container images


Navigate to your registry in the portal and select Repositories, then select the hello-world repository you created with docker push.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/519d292d-904e-42c5-8c5c-bdaa22cd4886)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c633c4e5-f49d-47ed-a359-e2ed5597e0ab)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/22ec11a7-143f-42f9-a08a-667e686b5e47)


### Run an image from the registry

Now you can pull and run the hello-world:v1 container image from your container registry by using docker run

```
docker runs <login-server>/hello-world:v1
docker run newregistryapl.azurecr.io/hello-world:v1
```













