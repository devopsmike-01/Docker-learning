# Docker Networking Overview
Docker networks allow containers to communicate with each other, the host system, and external systems. It provides various network drivers to cater to different use cases. Below is a detailed explanation of Docker's network types and respective commands with use cases and examples.

![Docker Network](https://media.geeksforgeeks.org/wp-content/uploads/20230419172809/Docker-network-1.webp)

## Types of Docker Networks

1. **Bridge Network (Default)**

The bridge network is Docker's default network driver for containers.
Containers in the bridge network can communicate with each other using container names or IP addresses but are isolated from the host network.

#### Commands
```
docker network ls  # List all networks
docker run -d --name container1 --network bridge nginx
docker run -d --name container2 --network bridge httpd
docker inspect container1
docker inspect container 2
docker exec -it container1 bash
apt update -y
apt-get install iputils-ping -y  
ping container 2

```
#### Explanation

- `docker network ls`: Lists all available Docker networks.
- `docker run`: Launches two containers (`nginx` and `httpd`) on the default bridge network.
- `docker exec`: Verifies communication between the two containers using ping.

*Use Case*: The bridge network is suitable for running multi-container applications on a single Docker host.


2. **Host Network**
The container shares the host's network stack and uses the host's IP address and ports. The container does not get its own IP address.

#### Commands
```
docker run -d --name container3 --network host nginx
curl http://public_ip
```
#### Explanation
- The `nginx` container runs using the host network.
- The curl command accesses `nginx` at http://public_ip since the container shares the host's network.

*Use Case*: The host network is ideal for performance-sensitive applications, such as proxies or applications that need direct access to the host's network stack.

3. **None Network**

Containers are isolated without any network interfaces.
No network communication is possible unless explicitly configured.

#### Commands
```
docker run --rm --network none alpine ping google.com
```

#### Explanation
- The `alpine` container is started with the `none` network driver.
- The `ping` command fails as the container lacks network access.

*Use Case*: The none network is useful for running offline tasks or applications that do not require network communication.


## Managing Docker Networks

#### Commands

- List all networks
```
docker network ls
```

- Inspect a network:
```
docker network inspect <network-name>
```

- Remove a network:
```
docker network rm <network-name>
```

**Use Case**: These commands help manage and monitor Docker networks to ensure proper configuration and troubleshoot connectivity issues.


## Managing Containers within a Docker Network

Docker allows fine-grained management of container connections within networks. Below are the key actions you can perform.

1. **Connecting a Container to a Network**

You can connect an existing container to a Docker network.

**Command**
```
docker network connect <network-name> <container-name>
```

**Example**
```
docker network connect app-network web
```

**Explanation**
The `web` container, originally not part of `app-network`, is now connected to it.
This enables web to communicate with other containers in `app-network`.

**Use Case**: Useful when a container must access services on another network without restarting.

2. **Disconnecting a Container from a Network**

You can remove a container from a Docker network.

**Command**
```
docker network disconnect <network-name> <container-name>
```
**Example**
```
docker network disconnect app-network db
```
**Explanation**:
- The `db` container is now isolated and cannot communicate with containers in `app-network`.

**Use Case**: Useful for debugging or enforcing network segmentation in multi-container applications.

3. **Inspecting a Container’s Network Connections**

You can view a container's network details, including the networks it is connected to.
**Command**
```
docker inspect <container-name>
```
- Example
```
docker inspect web
```

**Explanation**
Outputs detailed information about the web container, including its IP address, network interfaces, and connected networks.

**Use Case**: Useful for troubleshooting connectivity or verifying network configurations.

4. **Listing All Containers in a Network**

You can view all containers connected to a specific network.

**Command**
```
docker network inspect <network-name>
```
**Example**

docker network inspect app-network

**Explanation**
- Displays all containers connected to app-network, their IP addresses, and other configuration details.

**Use Case**: Helps monitor and manage container membership in networks.

5. **Running a Container on a Specific Network**

When starting a container, you can directly specify the network it should use.

**Command**
```
docker run --rm --name <container-name> --network <network-name> <image>
```
**Example**
```
docker run --rm --name test-app --network dev-network alpine ping web
```
**Explanation**
- Launches the test-app container on dev-network and tests connectivity to the web container.
**Use Case:** Streamlines workflow by ensuring containers are part of the correct network at startup.


## Docker Network Excersises 

1. **Scenario 1**

Deploying a web application and its database using a custom network.

**Commands**

- Create a custom network
```
docker network create app-network
```

- Launch the containers
```
docker run -d --name web --network app-network nginx
docker run -d --name db --network app-network postgres
```

- Verify connectivity
```
docker exec -it web ping db
```
**Explanation**:

The `app-network` isolates the application from other networks.
Containers (`web` and `db`) communicate securely within the `custom` network.
**Use Case**: This setup is ideal for multi-container applications requiring isolated communication between components (e.g., a web server and a database).

2. **Scenario 2**

Isolating Development and Testing

- Create separate networks for development and testing environments:
```
docker network create dev-network
docker network create test-network
```

- Launch containers for each environment:
```
docker run -d --name dev-app --network dev-network my-app-image
docker run -d --name test-app --network test-network my-app-image
```