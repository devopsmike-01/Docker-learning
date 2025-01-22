
# Step-by-Step Demo: Dockerfile with Ubuntu Base Image

## Set up a directory structure

1. Create a project directory
```
mkdir ubuntu-dockerfile-demo
```
Get into the directory

```
cd ubuntu-dockerfile-demo
```
2. Create a basic script that simulates an application, called `app.sh`
```
vi app.sh
```
## Write the application code

1. Add the following content to `app.sh`
```
#!/bin/bash
echo "Welcome to the Ubuntu-based Docker container!"
echo "Today's date is: $(date)"
echo "The container is running on: $(uname -a)"
```
2. Make the script executable
```
chmod +x app.sh
```

## Create a Dockerfile

1. Create a file named `Dockerfile` in the directory:
```
vi  Dockerfile
```
2. Add the following commands to the `Dockerfile` to demonstrate a variety of Dockerfile features

```
FROM ubuntu:20.04
LABEL maintainer="DevOpswithMike"
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    vim \
    tzdata && \
    apt-get clean
RUN ln -sf /usr/share/zoneinfo/UTC /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata
WORKDIR /usr/src/app
COPY app.sh .
RUN chmod +x app.sh
EXPOSE 8080
CMD ["./app.sh"]

```

- **FROM**: Specifies the base image (`ubuntu:20.04`).
- **ENV**: Sets environment variables (`DEBIAN_FRONTEND=noninteractive` prevents interactive prompts during package installation).
- **RUN**: Executes commands during the image build process (e.g., updates and installs packages, sets up configurations).
- **WORKDIR**: Sets the working directory inside the container.
- **COPY**: Copies files from the host to the container.
- **LABEL**: Adds metadata about the image.
- **EXPOSE**: Declares a port for the container to listen on.
- **CMD**: Specifies the default command to run when the container starts.


## Build the Docker Image 

1. Run the following command to build the image

```
docker build -t ubuntu-demo-app .
```
