# Create a Node.js Web App and deploy it to Azure
We will create a Node.js app and deploy it to azure. To do so we will save some time by working with some sample code.

- we will clone a repository containing our Node.js app
- we will create the web app, we will utilise the command `up` that will give us some smart defaults for resource group and price plan 
- show how to manage the app

Let's begin

## Getting the sample app

First step is localising the sample app and clone its repository, like so:

```
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```
Navigating into `nodejs-docs-hello-world` and running `npm start`, tells us the app is up and running on `http://localhost:1337`. Navigating there in our browser shows us the following:

![](/assets/Screen Shot 2019-01-10 at 23.23.12.png)

## creating the web app

Let's now create the web app using our `azure cli`. We need to type the following:

```
az webapp up -n <app_name>
```
Let's give it a unique name. This will create the following for us:
- a default resource group
- a default price plan
- set the deployment type to `zip deploy`

This will attempt to create the default `resource group` and the default `price plan` but will most likely say they already exist and carry on creating the web app. This might take a little while

The command above will output a JSON response containing the key `app_url`. Take that value and enter the URL in your browser. It should look like this:
![](/assets/Screen Shot 2019-01-10 at 23.31.00.png)

## manage the app
of course we know we have gone for a quick and dirty way of creating our app and deployed it to azure. How do we change in the source code and redeploy it?   


