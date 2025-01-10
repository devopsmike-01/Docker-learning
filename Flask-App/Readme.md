# Instruction to deploy this Python Application

1. Create the python application using the `app.py` codes

```
import os
from flask import Flask
app = Flask(__name__)

@app.route("/")
def main():
    return "Welcome to Docker Class at JJTECH!"

@app.route('/how are you')
def hello():
    return 'I sure do good prof. How about you?'

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)                            
```

2. Run the application on your host to test if working

```
python3 app.py
```

3. Create a Docker file 
```
vi Dockerfile 

```

4. Paste the following docker file and save with `wq!`
```
FROM ubuntu:20.04

RUN apt-get update && \
    apt-get install -y python3 python3-pip && \
    pip3 install flask

WORKDIR /app

COPY app.py .

CMD ["python3", "app.py"]
```


5. Build the image, tagging it with your dockerhub username e.g `michaelgwei86/practice-flask-app:v1` 
```
docker build -t michaelgwei86/practice-python-app:v1 .
```

6. Create a container from your above image.

```
docker run --name practice-flask-image -d -p 8080:8080 b3c0284794c4
```