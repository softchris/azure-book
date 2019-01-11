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

## create a resource group
we can either use an existing resource group or create a new one. We create a new one by typing:

```
az group create --name cosmosgroup --location "West Europe"
```

## create a cosmos db account
to create our cosmos db account we need to type the following:

```
az cosmosdb create --name cosmosdbchris --resource-group cosmosgroup --kind MongoDB
```
as you can see we need to specify `--resource-group` and `--kind`. CosmosDB is more like a service that allows us to choose from several types of databases.

This will take a while so go have a warm beverage :)

Once it's done it should look something like this:
```
 "writeLocations": [
    {
      "documentEndpoint": "https://<name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "cosmosdbchris-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
```

## set up the apps connection string with the CosmosDB one
Now it's time to go into our app and set the connection string to our MongoDB database in the cloud and try to see if our app is able to run locally but connect to our cloud database. We need to do the following to make that happen:

- retrieve the key
- use the key and set the connection string in our app
- test the app locally

### retrieve key
We do that by using the `list-keys` command

```
az cosmosdb list-keys --name cosmosdbchris --resource-group cosmosgroup
```
This will give us a JSON response back. We want the value for the key `primaryMasterKey` so copy that one


### use the key and set the connection string in our app
Inside of the apps `config/env` directory there is a file called `local-production.js`. This is currently specified in the `.gitignore` file so this is only about a local test, we will need set the connection string differently once the app is in the cloud. For now bare with me and let's try to set up the connection string for our local test:

```
module.exports = {
  db: {
    uri: 'mongodb://cosmosdbchris:<primary_master_key>@cosmosdbchris.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false'
  }
};
```

### test the app locally


At this point we need to create a production build of the app. If we have a global install of gulp we can just type:

```
gulp prod
```
If we have a local install we type:

```
node ./node_modules/gulp/bin/gulp.js
```

Now let's run the app with the following command:

```
NODE_ENV=production node server.js
```

The app should now be found at ip `http://0.0.0.0:8443`. Yes, it's there and it's actually using our Cosmos DB instead of the local one. We can see that if we go to Azure portal, find our resource group `cosmosgroup`, then click on our cosmosdb instance and click `Data Explorer` we will see this:
![](/assets/Screen Shot 2019-01-11 at 22.12.19.png) 








