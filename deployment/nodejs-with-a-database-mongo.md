# Deploy a Node.js + Mongo DB database
This assumes you have a MongoDB server running locally. 

We will take do the following that will result in our app being deployed, database and all:

- **set up and install MongoDB**, so we can test our app locally, no need if we already have MongoDB installed
- **clone a sample app** from github
- **create a resource group**, we will need this logic group for our web app and database account
- **create a CosmosDB account**, so we have a database to connect to
- **set up the apps connection string with the CosmosDB one**, so we can test that when we run the app locally that it is able to connect to our CosmosDB instance on Azure
- **create a deployment user**, we will need that one to do any kind of deploy
- **create a service plan**, so we know how we get billed for our created resources and usage of them
- **create a web app**, this will create a space for our app to live in, we will need to push code here
- **set db app settings**, to ensure our app is running towards the CosmosDB instance when running in the cloud
- **push the app to azure**, this is to make sure our code ends up in azure
- **manage changes and redeploy**, we will learn how to change code and publish the changes




## Set up and install MongoDB
If you already have MongoDB installed and working, skip this step otherwise keep reading.

Go to the download page [Download page](https://www.mongodb.com/download-center/community?jmp=docs) and click `Download`

Thereafter double click the installation file and find the resulting unpacked directory. 

### Set the path

Navigate to the directory and run `pwd`. Thereafter set it up in the PATH variable, like so:

/Users/chnoring/Downloads/mongodb-osx-x86_64-4.0.5

```
export PATH=<mongodb-install-directory>/bin:$PATH
```

### Create data directory and set access rights

Thereafter we need to set up the data directory where MongoDB will read and write from. We do that with:

```
mkdir -p /data/db
```

Then you need to ensure your data directory has the correct read/write access. Type the following:

```
sudo chmod -R go+w /data/db
```

### Start the engine

Thereafter start `mongod`, this will start the background process

## Get the sample app

Let's get the sample app

```
git clone https://github.com/Azure-Samples/meanjs.git
```

Thereafter type the following:

```
cd meanjs
npm install && npm start
```

Now you should get the following if you navigate to `http://localhost:3000`
![](/assets/Screen Shot 2019-01-11 at 00.53.10.png)

