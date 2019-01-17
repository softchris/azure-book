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
It does the following:

- **Selects an OS image**,   In short we base the OS on Ubuntu and a release called alpine, that has Node.js pre installed. 
- **Creates a directory**, with the following command `mkdir -p /usr/src/app
`
- **Copies all the files**, from `./app/` to `/usr/src/app/`
- **Sets working directory**, to `/usr/src/app`
- **Installs our node dependencies**, using `npm install`
- **Starts our app**, using `node /usr/src/app/index.js
`
## Build the image

We can use the `docker build` command to build an image. The exact command we will need to use is:
> docker build ./aci-helloworld -t aci-tutorial-app

The above command looks for the Dockerfile in the directory `/aci-helloworld` and creates an image called `aci-tutorial-app`

We can see our created image if we run the following command:

> docker images

## Test the image, by instantiating a container
Now that we have an image, we can create a container from it, using `docker run`. The full command looks like the following:

> docker run -d -p 8080:80 aci-tutorial-app

Let's look at the arguments:

- `-d`, this tells the container to run in the background
- `-p`, this allows us to map ports, the argument value should be interpreted like this [external port]:[containers internal port]

We can see that the external port is 8080, which means we can navigate to 
> http://localhost:8080
to ensure our application works.

This is the image we get, so I would say our container is working:

![](/assets/Screen Shot 2019-01-17 at 01.39.42.png)  

With the following command we can list all the running containers:

> docker ps
It should present the following result:
![](/assets/Screen Shot 2019-01-17 at 01.42.32.png)

