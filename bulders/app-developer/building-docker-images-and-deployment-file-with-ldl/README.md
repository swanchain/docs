# Building Docker Images and Deployment file with LDL

This guide will walk you through the process of building and pushing a Docker image, and then creating a deployment file using Lagrange Definition Language (LDL) to deploy your application on the Swan Chain network.

### Background

When you deploy your application to SwanChain using the Swan SDK, you need to upload a project with a Dockerfile or a deploy.yaml to GitHub. This article describes how to create a project with a Dockerfile or a deploy.yaml file and upload it to GitHub.

### Key Concepts

* A `Dockerfile` is a text document containing commands to assemble a Docker image.
* A `deploy.yaml` file, written in [Lagrange Definition Language (LDL)](creating-deployment-files-with-ldl.md), specifies deployment details for your Applications. This gives you more flexibility to add multiple dependent services and more configurations to your application

### Building and Pushing Your Docker Image

1. **Download the template:** Download the [docker-application-template](https://github.com/swanchain/docker-application-template/archive/refs/heads/main.zip)
2. **Navigate to the app folder:**

```
cd docker-application-template/app
```

3. **Modify the application code:** Edit the `main.py` file.&#x20;

For example:

```python
from fastapi import FastAPI
from datetime import datetime

app = FastAPI()

@app.get("/")
def read_root():
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    return {"Hello!"  f"Today is - {current_time}"}
```

4. **Push your code to GitHub**

{% hint style="info" %}
You’re now ready to use your repo URL and the Swan SDK to [deploy your first application](../deploying-with-swan-sdk.md)!
{% endhint %}

***

_If you believe the Dockerfile alone cannot meet your complex deployment requirements, the next steps will guide you in preparing and creating a deploy.yaml file yourself._

5. **Build the Docker image:** Navigate to the root folder and run:

```
cd docker-application-template
docker build --platform linux/amd64 --tag <username>/<repo>:<tag> .
```

6. **Push to container registry:**

```
docker push <username>/<repo>:<tag>
```

### Creating Deployment Files with LDL

7. **Create a new project folder:**

```
mkdir my-swan-app-with-ldl
cd my-swan-app-with-ldl
```

8. **Create the deploy.yaml file:**&#x20;

Create a `deploy.yaml` file in the root of the folder with the following content:

```yaml
version: "2.0"
services:
  hello-world:
    image: <username>/<repo>:<tag>
    expose:
      - port: 7860
        as: 80
deployment:
  hello-world:
    lagrange:
      count: 1
```

Note: Replace `<username>/<repo>:<tag>` with your Docker image information.

9. **Push the LDL project to GitHub**

### Next Steps

With your Docker image built and pushed, and your LDL deployment file created, you're now ready to deploy your application on the Swan Chain network using Swan SDK. Refer to the [Swan SDK documentation](../deploying-with-swan-sdk.md) for the next steps in the deployment process.

For more information on customizing your deployment settings with LDL, check out the [LDL documentation.](creating-deployment-files-with-ldl.md)
