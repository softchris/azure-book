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


## create a deployment user
Let's start focusing on getting the app to the cloud instead of running it locally. First thing we need to do is to get a deployment user. If you have created on before you might use that again, otherwise we need to type the following:

```
az webapp deployment user set --user-name <username> --password <password>
```
The user name needs to be globally unique. 

## create a service plan
this is so we know how we will be billed for the resources under this resource group. Let's create a service plan with the following command:
```
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1 --is-linux
```

## create a web app
Now it's time to create the web app, where our app will finally live. Let's create it with the following command:

```
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
```

Now we have created our web app and by us specifying `--deployment-local-git` means we can use git to push the app

## set db app settings
Remember how we had set the connection string before? Well we had done so in a file that we didn't aim to check in. What we can do instead is to set the connection string with an azure cli command instead:

Let's use the following command:

```
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

Now to access this app setting key in Node.js. We just need to refer to `process.env.MONGODB_URI`. Therefore let's go into `config/env/production.js`, note a code piece that looks like this:

```
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```
The above means we are already set up and ready to connect to MongoDB. We don't actually need to change anything.

## push the app to azure
Next step is to push the app to azure. We do that with the following command:

```
git remote add azure <deploymentLocalGitUrl-from-create-step>
```
The above means that we need to find the value of key `deploymentLocalGitUrl` that we got from creating our web site. Let' run the command to have the remote branch point correctly.

All done? Good!

Ok let's actually push our code to azure with the following command:

```
git push azure master
```

go visit your site on the URL indicated by the key `hostNames` from when you generated the web site. ( this might take a little while the first time, it needs to download all npm libraries etc. )

After all that wait, it should look like this:
![](/assets/Screen Shot 2019-01-11 at 22.58.04.png)

## manage changes and redeploy

This is quite simple. Do the changes you need to in the code and then run:

```
gulp prod
NODE_ENV=production node server.js
```

Thereafter commit your changes:

```
git commit -am "added article comment"
git push azure master
```

After that your app should be updated

## Summary
We learned not only to create a web app but also to create a database and how to set the connection string. 

We've learned we could use the following `az cosmosdb create` to create a CosmosDB database and also to type `az cosmosdb list-keys` to get the keys we would need to construct our connection string.

Furthermore we learned how we could update the web apps app settings with the following command:

```
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```
The above is definitely possible to do through the portal UI as well but if we want to be terminal hackers, this looks cooler ;)

We've also learned that we can access app settings keys in our node.js web app by typing the following:

```
process.env.<appkey>
```

We've thereby learned the basic of what you would expect of any app. Of course you would need to learn about logging to track and trace errors and security to feel really comfortable as a developer in the cloud.  

### Further reading

This is the tutorial I was following from Microsoft Docs [Node.js + MongoDB tutorial](https://docs.microsoft.com/en-gb/azure/app-service/containers/tutorial-nodejs-mongodb-app)






