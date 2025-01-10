# Instruction to deploy this Nodejs Application

1. Install `git` and clone the following repository to extract all the necessary application files.

- Installing git

```
sudo yum install git -y
```
- Clone the repository 

```
git clone https://github.com/devopsmike-01/Docker-learning.git

```

2. Move to the application Directory.

```
cd Nodejs-App

```

3. Build the image, tagging it with your dockerhub username e.g `michaelgwei86/practice-nodejs-app:v1` 

```
docker build -t michaelgwei86/practice-nodejs-app:v1 .
```

4. Create a container from your above image.

```
docker run --name practice-nodejs-container -d -p 8080:8080 b3c0284794c4
```