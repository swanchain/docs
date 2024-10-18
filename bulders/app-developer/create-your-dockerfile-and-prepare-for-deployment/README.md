# Create Your Dockerfile and Prepare for Deployment

Welcome to the Swan App Developer Guide! This page will walk you through the process of building your own Swan-SDK Deployable Dockerfile. And if you're a pro, you will learn how to create a deployment file with LDL.

#### You will learn

* How to customize your Docker image base on our template
* What is LDL and how to create a deployment file with it

## Part 1: Build Your Own Docker Images Based on Our Template

> Note: You can skip this part if you already have your Docker image with your application and move to [Deploy with swan sdk](../deploying-with-swan-sdk.md)

Building your Docker image is the first step in deploying custom applications on the Swan Chain network. Here's how you can do it:

1. Download the [docker-application-template](https://github.com/swanchain/docker-application-template/archive/refs/heads/main.zip)
2. Navigate to the app folder under root of the downloaded folder:

```
cd docker-application-template/app
```

3. Change the `main.py` file to your application code. In this demo, we will just change the return message:

```python
from fastapi import FastAPI
from datetime import datetime

app = FastAPI()

@app.get("/")
def read_root():
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    # return {"Hello World!" f"Current time - {current_time}"} 
    return {"Hello!"  f"Today is - {current_time}"}
```

4. Push your code to your **public** GitHub repository.

```sh
git init
git add .
git commit -m "My First Swan App"
git branch -M main
git remote add origin <your-github-repo>
git push -u origin main
```

👏 And congratulations! You are now ready to deploy your application on the Swan Chain network via SwanSDK by following [deploy with swan sdk](../deploying-with-swan-sdk.md)

***

## Part 2: Creating Deployment Files with LDL (Optional)

🤔 A single docker file is not enough for you? No worries! You can create a deployment file with LDL to customize your deployment settings. First thing first, you need to know what is LDL and why do we need it:

```Markdown
The Lagrange Definition Language (LDL) is a YAML-based configuration language designed to define deployment requirements
on the Swan Chain network. LDL files (deploy.yaml) outline how your application should be deployed and run on Swan
Chain.

Using a `deploy.yaml` file allows you to configure deployment settings, networking, dependencies, and scaling policies,
providing full control over your application’s operation.

And with using `deploy.yaml` file, you will only need the file to deploy your application on the Swan Chain network.

```

By using that, you will first need to push your Docker Image to the Docker Hub. Here is how you can do it:

1. Build the Docker image (Base on the above Dockerfile example and navigate to the root of the folder):

```sh
cd docker-application-template
docker build --platform linux/amd64 --tag <username>/<repo>:<tag> .
```

2. Push your container registry:

```
docker push <username>/<repo>:<tag>
```

> Note: After completing the above process, you can view the image information on the [docker hub](https://hub.docker.com)

3. Build your deployment file with LDL. Create a new folder out of the `docker-application-template` folder, navigate to it:

```sh
mkdir my-swan-app-with-ldl
cd my-swan-app-with-ldl
```

Create a `deploy.yaml` file in the root of the folder and add the following content:

> Note: Replace `<username>/<repo>:<tag>` with your Docker image information.

> If you have dockerfile and deploy.yaml in the same project, deploy.yaml will be prioritized for use with

```
version: "2.0"
services:
  hello-wold:
    image: <username>/<repo>:<tag>
    expose:
      - port: 7860
        as: 80
deployment:
  hello-wold:
    lagrange:
      count: 1
```

4. Push your code to your **public** GitHub repository (should be different from the above one).

```sh
git init
git add .
git commit -m "My First Swan App with LDL"
git branch -M main
git remote add origin <your-another-github-repo>
git push -u origin main
```

With the above steps, you have successfully created your own Docker image and deployment file with LDL. You can now [deploy your app with swan sdk](../deploying-with-swan-sdk.md)

LDL is a powerful tool that allows you to customize your deployment settings. You can learn more about LDL [here](creating-deployment-files-with-ldl.md)
