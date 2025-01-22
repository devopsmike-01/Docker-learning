# FROM uses an official Ubuntu base image
FROM ubuntu:20.04


# LABEL Adds a metadata label
LABEL maintainer="DevOpswithMike"

# ENV Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# RUN executes commands to Update package lists and install dependencies
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    vim \
    tzdata && \
    apt-get clean

# RUN Set the time zone (example of configuring the container)
RUN ln -sf /usr/share/zoneinfo/UTC /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# WORKDIR creates a directory inside the container
WORKDIR /usr/src/app

# COPY Copies the application script into the container
COPY app.sh .

# RUN Set permissions for the script
RUN chmod +x app.sh

# EXPOSE Expose a port (even though the script doesn't use networking)
EXPOSE 8080

# CMD Define a default command to run the script
CMD ["./app.sh"]