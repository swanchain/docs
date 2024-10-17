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

<figure><img src="https://docs.lagrangedao.org/~gitbook/image?url=https%3A%2F%2Fgithub.com%2Fuser-attachments%2Fassets%2F188d1415-747f-4c0e-a053-43ea587ea5fd&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=9ffc8545&#x26;sv=1" alt=""><figcaption></figcaption></figure>
