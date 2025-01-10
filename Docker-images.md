# Docker images

## What is a docker image?
Images are multi-layered self-contained files that act as the template for creating containers. They are like a frozen, read-only copy of a container. Images can be exchanged through registries.
Containers are just images in running state. When you obtain an image from the internet and run a container using that image, you essentially create another temporary writable layer on top of the previous read-only ones

![Docker Images](https://media2.dev.to/dynamic/image/width=1000,height=500,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Frdlipe7r7g8fenj7ahg4.png)

### Key  image features
- A docker image is a snapshot of the filesystem + some metadata 
- Identified by unique hex IDs
- may be tagged(severally) with a human-friendly name

## Managing Docker images

1. **Fetching an image from Dockerhub (`docker pull`)**
- Images are pulled from Docker hub
- Docker initiates a pull of the latest image
- You can pull a specific version using tags
- You can also pull an image from a Registry
---
To pull a  latest  image
   ```bash
   docker pull ubuntu
   ```

---
To pull a tagged image
   ```bash
   docker pull ubuntu:14.04
   ```
---

   ```bash
   docker pull registry.example.com/username/ubuntu:14.04
   ```
--- 

2. **Listing downloaded images**

   ```bash
   docker images
   ```
---

3. **Building Docker images**

You can create an image first by downloading a template or sample image from the docker repository. 

For example

   ```bash
   docker pull ubuntu
   ```
---

Secondly, 
Once you have a Dockerfile, you can build an image from it using docker build. The basic form of this command is:

   ```bash
   docker build -t image-name /path
   ```
---

   ```bash
   docker build -t image-name .  //building in the current directory
   ```
---

4. **Tagging Docker images**

Tagging an image is useful for keeping track of different image versions:
- Usage

   ```bash
   docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
   ```
---

- Example usage

   ```bash
   docker tag 6efc10a0510f:stablev1 nginx:stablev2
   ```
---

5. **Referencing Docker images**

-  You can reference by `image ID`: for example `693bce725149`
-  You can reference by `Name`: for example `hello-world` (defaults to :latest tag)
- You can reference by `Name + Tag`: for example `hello-world:latest`

6. **Removing images**
The `docker rmi` command is used to remove images:

```
docker rmi  <image name>
```
---


It is also possible to remove images by their ID instead:

```
docker rmi 693bce725149
```
---

You can also remove the images using the first three characters of the image id

*Remove All Images with No Started Containers*

An image cannot be removed locally if it is being `RUN` by a container. 

To remove all local images that have no started containers, you can provide a listing of the images as a parameter:

```
docker rmi $(docker images -qa)
```
---

If you want to remove images regardless of whether or not they have a started container use the force flag (-f):

```
docker rmi -f $(docker images -qa)
```
---

7. **Searching images**

You can search Docker Hub for images by using the search command:
```
docker search <term>
```
---

For Example

```
docker search nginx
```
---

8. **Searching images**

The output is in JSON format

```
docker inspect <image>
```
---

9. **Saving an image**

```
docker save -o ubuntu.latest.tar ubuntu:latest
```
---

This command will save the `ubuntu:latest` image as a `tarball` archive in the current directory with the name `ubuntu.latest.tar`. 

This tarball archive can then be moved to another host, for example using `rsync`, or archived in storage.

Once the `tarball` has been moved, the following command will create an image from the file

```
docker load -i /tmp/ubuntu.latest.tar
```
---

10. **Pushing and image to Dockerhub**

Locally created images can be pushed to Docker Hub or any other docker repo host, known as a registry. Use `docker login` to sign in to an existing docker hub account.
```
docker login
```
---
- Enter: `Username`
- Enter: `Password`

You can then tag and push images to the registry that you are logged in to. Your repository must be specified as `/username/reponame:tag`.

For example
```
docker tag mynginx cjsimon/mynginx:latest
```
---
