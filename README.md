
# Flask Containerized App

This Flask containerized application displays a simple "Hello World" message.

![docker](https://user-images.githubusercontent.com/82409763/221435713-9dfc4d09-550b-4369-8eed-4054761579a0.jpg)

# Installing Docker on Amazon Linux server

### Pre-requisites

1. Amazon Linux EC2 Instance

## Installation Steps

1. Run the following commands to install Docker

```sh
yum install docker -y
docker --version 
```   

## Starting the Docker Services
1. Run the following commands to start Docker services
```sh
# start docker services
service docker start
service docker status
``` 
# Project setup

### Create Project Directory
1. Create a new directory for your project using the following command:

```sh
mkdir myflaskapp
cd myflaskapp
```

### Create Dockerfile
1. Create a new file in directory called "Dockerfile"
Add the following content to the file:

```sh
# Use an official Python
FROM python:3.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]

```

### Create a requirements.txt file

1. Create a new file in project directory called "requirements.txt"
2. Add this package in the file, one per line

```sh
Flask
```

### Create Application

1. Create a new file in project directory called "app.py"
2. Add your application code to this file

```sh
from flask import Flask
import os

app = Flask(__name__)

@app.route("/")
def hello():
    name = os.getenv("NAME", "World")
    return f"Hello, {name}!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=int(os.environ.get('PORT', 80)))

```

### Build and test the Docker image

1. Run the following command to build the Docker image:

```sh
docker build -t myapp .
```

2. Run the following command to check the available images

```sh
docker images
```

3. Run the following command to start a container from the image:

```sh
docker run -d -p <port>:80 --name myfirstapp myapp:latest
```

4. Run the following command to check running container

```sh
docker ps
```
5. Testing in the Browser

To access your Application, simply pick the public IP address of the instance and add the port number to the end of the URL
For example http://your-ip:port-number


### Troubleshooting

1. If you encounter an error while accessing your application on EC2, it may be due to the security group settings, so please ensure that the port number you are using to run the container is allowed in the inbound rules of the EC2 instance's security group.
