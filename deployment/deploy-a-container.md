# Deploy a container

> Azure Container Instances enables deployment of Docker containers onto Azure infrastructure without provisioning any virtual machines or adopting a higher-level service.

- **Clone** application source code from GitHub
- **Create** a container image from application source
- **Test** the image in a local Docker environment

You will ned the following installed
- Docker 
- Azure CLI [install](https://docs.microsoft.com/cli/azure/install-azure-cli)

- Docker Engine

## Clone the application

> git clone https://github.com/Azure-Samples/aci-helloworld.git


In the source code there is docker image with the following content:

```
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/ /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

