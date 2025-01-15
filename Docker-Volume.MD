# Docker Volumes
Docker networks allow containers to communicate with each other, the host system, and external systems. It provides various network drivers to cater to different use cases. Below is a detailed explanation of Docker's network types and respective commands with use cases and examples.

![Docker Network](https://miro.medium.com/v2/resize:fit:1200/1*xONk464vW-xNYxzE_HsSkw.png)

## Key Features of Docker Volumes

1. **Data Persistence**

Volumes allow data to persist even after the container is stopped or removed. This is crucial for applications that require data retention.

2. **Decoupling Data from Containers**

Volumes enable you to separate data from the lifecycle of a container, allowing for easier management and upgrades.

3. **Sharing Data Between Containers**

Multiple containers can access the same volume, facilitating data sharing and collaboration between them.

4. **Performance**

Volumes can offer better performance compared to storing data in the container's writable layer, especially for I/O operations.

5. **Backup and Restore**
Volumes can be easily backed up or migrated, making it simpler to manage data across different environments.


## Managing Docker Volumes

1. **Creating a Volume**

To create a Docker volume, use the following command

```
docker volume create my_volume
```

2. **Listing Volumes**

To view all Docker volumes on your system, use

```
docker volume ls
```
3. **Inspecting a Volume**

To get detailed information about a volume, run

```
docker volume inspect my_volume
```

4. **Removing a Volume**

To delete a volume, ensure no containers are using it, then run

```
docker volume rm my_volume
```

5. **Mounting Host Directory to Volume**

To bind a specific directory on the host to the a docker volume for use with a container volume,

```
docker run -v <host_volume:container_volume>
```

For example

```
docker run --rm -it -v my-data-volume:/app/data alpine sh
```

## Example Steps

1. Create a volume:
```
docker volume create my_volume
```
2. Run the first container with the volume mounted to the host directory

```
docker run -d -v /home/ec2-user:/data --name container1 alpine tail -f /dev/null
```

- `-d`: Runs the container in `detached` mode.
- `-v` `/home/ec2-user:/data`: Mounts the host directory `/home/ec2-user` to the container directory `/data`.
- `--name` `container1`: Names the container for easier reference.
- `tail -f /dev/null`: Keeps the container running.

3. Exec into the first container

```
docker exec -it container1 sh
```

4. Create a directory in the volume and a sample file (`demo.txt`)

Inside the container shell, run the following commands
```
mkdir /data/my_directory
vi demo.txt
```
Past this in the demo.txt file and save it 
```
"This is a sample file."
```

5. Run a second container with the same volume
```
docker run -d -v /home/ec2-user:/data --name container2 alpine tail -f /dev/null
```

6. Exec into the second container

```
docker exec -it container2 sh
```

7. Access the volume contents

Inside the second container shell

```
ls /data/my_directory
cat /data/my_directory/demo.txt
```

8. Create new content in the second container with the same volume:

Inside the second container shell

echo "This is another sample file." > /data/my_directory/another_demo.txt



This example demonstrates how to create a Docker volume mounted to a host directory and use it between two containers. You can share and manipulate data easily across containers while keeping the data persistent on the host machine.



**Mounting a host directory `/home/ec2-user/data` directory as a persistent volume for `Jenkins` container**

- Create directory on host machine

```
mkdir -p /home/ec2-user/data
```
- Run the Jenkins Container with the Volume Mounted
```
docker run -d --name jenkins05 -p 8082:8080 -p 50050:50000 -v /home/ec2-user/data:/var/jenkins_home jenkins/jenkins:lts
```
- To confirm that the volume is mounted correctly

```
docker exec -it -u root <container_id> bash
```