# Dockerfile Instructions
- The instructions in a Dockerfile are executed in order from top to bottom. Each instruction creates a new layer on top of the previous one.
- The layers are committed when instructions like `CMD`, `COPY`, `ADD`, etc. are encountered.
- The FROM instruction initializes the base image and should be the first instruction in any Dockerfile.
- The order of instructions matters and determines the step-by-step build process of the image.

## Popular Dockerfile Instructions

- **FROM:** Sets the base image.
- **MAINTAINER:** Author/Maintainer information.
- **ENV:** Sets environment variable.
- **ADD:** Copies files to the image.
- **COPY:** Copies files or directories to the image
- **RUN:** Execute commands in a new layer
- **ENTRYPOINT:** Sets the primary command to be executed at container startup
- **CMD:** Default command to execute at container startup
- **USER:** Sets the user to use when running the image
- **WORKDIR:** Sets the working directory for future commands
- **ARG:** Allows ARG instructions to define build-time variables
- **ONBUILD:** Adds a trigger instruction to be executed when the image is built
- **STOPSIGNAL:** Sets the default signal to stop a container
- **HEALTHCHECK:** Set health check instructions
- **EXPOSE:** Exposes a port for users of the image
- **VOLUME:** Creates a mount point within the image

## Understanding each Dockerfile Instruction

### FROM Instruction
The `FROM` instruction specifies the base image from which the Docker image is built. Every Dockerfile must begin with a `FROM` statement, except for multistage builds where additional `FROM` instructions can appear later.

#### Use Cases
- Defining a base operating system or application image
- Building images in multiple stages

#### Example 1: Setting a Base Image
```
FROM ubuntu:20.04
```
This sets `ubuntu:20.04` as the base operating system for the Docker image.

#### Example 2: Using Multistage Builds
```
FROM node:14 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build  

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

This uses a multistage build to create a production-ready web application using Node.js and Nginx.

### LABEL Instruction 
The `LABEL` instruction was used to declare the author of the Dockerfile.

#### Use Cases
- Adding author information 
- Using LABEL for metadata instead

#### Example 1: Defining a Label 
```
LABEL maintainer="John Doe <john.doe@example.com>"
```
This declares `John Doe` as the Dockerfile maintainer.

### ENV Instruction
The `ENV` instruction sets environment variables inside the Docker container. These variables persist across build stages and runtime, allowing for flexible configurations.

#### Use Cases
- Defining environment variables for configuration
- Setting default values for apps

#### Example 1: Setting Environment Variables
```
ENV APP_NAME="MyApp" PORT=8080
```
This sets `APP_NAME` to `MyApp` and `PORT` to `8080` inside the container.

#### Example 2: Using Variables in Commands
```
ENV APP_HOME="/app"
WORKDIR $APP_HOME
```
This sets the working directory to `/app` using the environment variable `APP_HOME`.

### ADD Instruction
The `ADD` instruction copies files and directories from the host machine to the Docker container. Unlike `COPY`, it supports automatic extraction of compressed files `(.tar.gz)` and remote URLs.

#### Use Cases
- Copying files from the host system
- Extracting archives automatically

#### Example 1: Adding Files from Host to Container
```
ADD hello.txt /app/
```
This copies `hello.txt` from the current directory on the host to `/app` in the container.

#### Example 2: Extracting Archives
```
ADD archive.tar.gz /app/
```

This extracts `archive.tar.gz` into the `/app` directory inside the container.

#### Best Practice
Use `COPY` unless you specifically need archive extraction or `URL` downloads.

### COPY Instruction
The `COPY` instruction copies files and directories from the host machine to the Docker container. It is simpler and more predictable than `ADD`, making it the recommended way to copy files in most cases.

#### Use Cases
- Copying files and directories securely
- Avoiding unwanted archive extraction

#### Example 1: Copying Files from Host to Container
```
COPY config.json /app/
```
This copies `config.json` from the build context on the host to `/app` in the container.

#### Example 2: Copying a Directory
```
COPY . /app
```
This copies the entire current directory to `/app` in the container.

#### Best Practice
Always use `COPY` unless `ADD`'s extra features (like archive extraction) are explicitly required.

### RUN Instruction
The `RUN` instruction is used to execute commands in a new layer on top of the current image and commit the results. It is commonly used for installing software packages, configuring services, or running scripts during the Docker image build process.

#### Use Cases
- Install system packages
- Download dependencies
- Configure system settings

#### Example 1: Installing Packages
```
RUN apt-get update && apt-get install -y nginx
```

This updates the package list and installs nginx in the Docker container.

#### Example 2: Running a Script
```
RUN ./setup.sh
```

This runs the setup.sh script from the Docker build context.

### ENTRYPOINT Instruction
The `ENTRYPOINT` instruction specifies the main application or command that runs when the container starts. Unlike `CMD`, it cannot be easily overridden at runtime unless the container is run with additional arguments.

### Use Cases
- Setting the primary executable for the container
- Defining fixed services or commands

#### Example 1: Running a Web Server
```
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]
```
This sets nginx as the container's main process and prevents it from running in the background.

#### Example 2: Running a Custom Script
```
ENTRYPOINT ["/app/start.sh"]
```
This ensures start.sh is always executed when the container runs.

### CMD Instruction
The `CMD` instruction provides default arguments for the container. It is overridden if arguments are passed when running the container. Use it for commands that are optional or flexible.

### Use Cases
- Providing default commands or scripts
- Setting container startup configurations

#### Example 1: Default Message Output
```
CMD ["echo", "Hello, Docker!"]
```
This prints "Hello, Docker!" unless a different command is provided.

#### Example 2: Starting an App
```
CMD ["python3", "app.py"]
```
This runs app.py unless another script is specified at runtime.

### USER Instruction
The `USER` instruction specifies the user under which the container should run. Running containers as root is risky, so it’s recommended to use a non-root user.

#### Use Cases
- Improving container security
- Limiting system access

#### Example 1: Switching to a Non-Root User
```
USER appuser
```
This ensures the container runs processes as appuser, reducing security risks.

#### Example 2: Using a System UID
```
USER 1001
```
This uses the system user with UID 1001 to run processes.

### WORKDIR Instruction
The `WORKDIR` instruction sets the working directory for all subsequent `RUN`, `CMD`, `ENTRYPOINT`, and other instructions. It simplifies file path management during builds.

#### Use Cases
- Managing file paths
- Simplifying file structure

#### Example 1: Setting a Work Directory
```
WORKDIR /app
```
This sets /app as the default directory for any following instructions.

#### Example 2: Running Commands in a Specific Directory
```
WORKDIR /usr/src/app
RUN npm install
```
This installs npm packages from /usr/src/app.

### EXPOSE Instruction
The `EXPOSE` instruction informs Docker that the container listens on specified network ports at runtime. It does not publish ports automatically but serves as documentation and assists with port binding.

#### Use Cases
- Defining exposed service ports
- Documenting service configurations

#### Example 1: Exposing a Web Server Port
```
EXPOSE 80
```
This indicates the container listens on port 80, commonly used by web servers.

#### Example 2: Multiple Ports
```
EXPOSE 8080 443
```
This exposes both HTTP (8080) and HTTPS (443) ports.

### VOLUME Instruction
The `VOLUME` instruction creates a mount point with a specific path and allows persistent data storage outside the container. It’s useful for maintaining data consistency.

#### Use Cases:
- Data persistence
- Sharing files between containers

#### Example 1: Defining a Persistent Volume
```
VOLUME /data
```
This creates a volume at /data that persists even if the container is removed.

#### Example 2: Mounting Configuration Files
```
VOLUME ["/app/config"]
```
This stores configuration files outside the container for persistent use.

### ARG Instruction
The `ARG` instruction defines build-time variables that can be passed when running docker build. It’s useful for customizing builds without hardcoding values.

#### Use Cases
- Setting build-specific values
- Customizing environment variables

#### Example 1: Defining a Build Argument
```
ARG VERSION=1.0
```
This defines a build argument VERSION, which can be overridden during the build.

#### Example 2: Using ARG in RUN
```
ARG VERSION
RUN echo "Building version $VERSION"
```

This prints the provided version during the build.

### ONBUILD Instruction
The `ONBUILD` instruction triggers specific commands when the image is used as a base for other Dockerfiles. It’s great for setting up pre-configured environments.

#### Use Cases
- Automating image extensions
- Pre-setting configurations

#### Example 1: Adding Files Automatically
```
ONBUILD ADD . /app
```
When another Dockerfile extends this image, the current directory is copied to /app automatically.

#### Example 2: Pre-Installing Dependencies
```
ONBUILD RUN npm install
```
This automatically installs dependencies if a derived image includes package.json.

### STOPSIGNAL Instruction
The `STOPSIGNAL` instruction defines the system signal used to stop the container gracefully. By default, Docker sends SIGTERM but can be customized.

#### Use Cases
- Handling graceful shutdowns
- Customizing termination signals

#### Example 1: Custom Stop Signal
```
STOPSIGNAL SIGQUIT
```
This ensures the container receives the SIGQUIT signal on shutdown.

#### Example 2: Managing App Shutdowns
```
STOPSIGNAL SIGTERM
```
This gracefully stops processes using the `SIGTERM` signal.








