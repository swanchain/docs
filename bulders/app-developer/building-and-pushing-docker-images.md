# Building and Pushing Docker Images

This step will walk you through building and pushing your Docker image to your container registry. This is useful to building custom images for your use case.

1. Clone the [docker-application-template](https://github.com/swanchain/docker-application-template):

```
git clone https://github.com/swanchain/docker-application-template.git
```

2. Navigate to the root of the cloned repo:

```
cd docker-application-template
```

3. Build the Docker image (example: filswan/helloworld:v1.0):

```
docker build --platform linux/amd64 --tag <username>/<repo>:<tag> .
```

4. Push your container registry:

```
docker push <username>/<repo>:<tag>
```

> Note: After completing the above process, you can view the image information on the [docker hub](https://hub.docker.com/repository/docker/filswan/helloworld/general), as shown below:

<figure><img src="https://private-user-images.githubusercontent.com/102578560/363108760-188d1415-747f-4c0e-a053-43ea587ea5fd.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjkyMjA1MTUsIm5iZiI6MTcyOTIyMDIxNSwicGF0aCI6Ii8xMDI1Nzg1NjAvMzYzMTA4NzYwLTE4OGQxNDE1LTc0N2YtNGMwZS1hMDUzLTQzZWE1ODdlYTVmZC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMDE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTAxOFQwMjU2NTVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hNzlmYzQ1ZTk5MWI2YzYyMGQxY2JjNTczNzhiYzFhZDE2MjQ4OTE0NDcyZGI0N2RjMzU5OGFlMThmNDIyYmFiJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.aiUiS95FsFgnzN5wTwl4pceWqofWeJ6XKjxwC-YjXGk" alt=""><figcaption></figcaption></figure>
