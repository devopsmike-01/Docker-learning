# Docker Registry and Docker Registry Commands
A Docker registry is a central repository for storing and distributing Docker images. Docker registries can be public or private, enabling teams to collaborate effectively by sharing images across different environments. Docker Hub is the most popular public registry, but private registries are also commonly used for secure and custom image storage.

![Basic Docker Taxonomy](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/container-docker-introduction/media/docker-containers-images-registries/taxonomy-of-docker-terms-and-concepts.png)

## What is a Docker Registry?
- A Docker Registry stores Docker images, which can be pulled by Docker clients or pushed from local Docker instances.
- A registry can host multiple repositories, and each repository can contain multiple versions (`tags`) of an image.
### Examples
- **Public Registries**: Docker Hub, Amazon Elastic Container Registry (ECR), Google Container Registry (GCR).
- **Private Registries**: Self-hosted registries using Docker's registry image or third-party services.

## Docker Registry Key Concepts

1. **Docker Image**
Bundles application code, dependencies, and runtime into a portable package.

2. **Repository**
A collection of related Docker images identified by `tags` (e.g., `nginx:latest` and `nginx:1.19`).

3. **Tag**
A version identifier for an image (e.g., `v1.0`, `stable`, or `latest`).

## Docker Registry Commands
Below is a list of Docker registry-related commands, their descriptions, use cases, and examples tailored with a Docker Hub username `michaelgwei86`:


![Docker Registry Workflow](https://media.geeksforgeeks.org/wp-content/uploads/20240513153832/Docker-hub-registry-768.webp)

1. **Docker Login** 

- Authenticate to a Docker registry (e.g., Docker Hub or private registry).

***Command***
```
docker login 
```
***Explanation***

* Prompts for `username` and `password`.
* Required before pushing or pulling private images.

2. **Docker Pull**

Download an image from a registry to your local machine.

***Command***
```
docker pull <image-name>:<tag>
```
***Example***
```
docker pull michaelgwei86/my-app:latest
```

***Explanation***

Downloads the `my-app` image with the latest `tag` from your Docker Hub account.

3. **Docker Push**

Upload a locally built image to a registry.

***Command***
```
docker push <image-name>:<tag>
```
***Example***
```
docker push michaelgwei86/my-app:v1.0
```
***Explanation***

Pushes the `my-app:v1.0` image to the `michaelgwei86/` Docker Hub repository.


4. **Docker Build and Push to Docker Hub**

Build a Docker image locally and push it to your Docker Hub account.

![Docker Build](https://miro.medium.com/v2/resize:fit:1400/1*OTLU6kstNgX7o_yR7HQVdA.png)

***Commands***

- Building the image

```
docker build -t <username>/<image-name>:<tag> .
```

- Pushing the image to Dockerhub

```
docker push <username>/<image-name>:<tag>
```

***Example***

- Building the image
```
docker build -t michaelgwei86/my-app:v1 .
```

- Pushing the image to Dockerhub

```
docker push michaelgwei86/my-app:v1

```
***Explanation***

- Builds an image from the current directory and tags it for your Docker Hub account.
- Pushes the built image to your Docker Hub repository.

5. **Docker Logout**

Log out of a Docker registry.

***Command***
```
docker logout
```
***Explanation***

- Removes stored authentication credentials for Docker Hub.
