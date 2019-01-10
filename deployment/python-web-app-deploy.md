# Create a Web App and deploy it to Azure
Here we will look how we can create a webapp and deploy it to azure. 

We need to do the following:

- **download** a sample app
- **run the app** locally just to ensure it works
- **configure a deployment user**, this deployment user is required for `FTP` and `local Git deployment` to a web app
- **create a resource group**, this is needed if you need a new logical grouping for what we are about to do
- **create a service plan**, we will need this to specify how we will be charged for this and what kind of container we will create
- **create the web app**, we will need to run a command to create the actual web app and we will need to state here how we deploy it, we will choose git deploy, more on that below
- **visit** the generated web site
- **push to the site**, using git deploy
- **manage updates**, let's learn how we can update our app and push changes

## Download and run sample app

Let's get a sample app first:

```
git clone https://github.com/Azure-Samples/python-docs-hello-world
cd python-docs-hello-world
```

if we want to run it locally we type:

```
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
FLASK_APP=application.py flask run
```
This should run the application and host it on `http://localhost:5000`

## Configure a deployment user
This deployment user will need to have a name that is globally unique

To create our deployment user we need to use the command `az webapp deployment`. Then we need to provide a user after the `--user-name` flag and a password after the `--password` flag. The password needs to be at least 8 characters long and contain two of the following three elements: letters, numbers, symbols. The full command we nee to run looks therefore like this:

```
az webapp deployment user set --user-name <username> --password <password>
```
Let's call the user `deployment-user-chris` and give it the password `test1234`. Thereby our command now looks like this:

```
az webapp deployment user set --user-name deployment-user-chris --password test1234
```

This should give you an output looking like this:
![](/assets/Screen Shot 2019-01-10 at 22.15.38.png)

## create a resource group
If you want a new group, type the following:

```
az group create \
    --name resourceForStorageAccount \
    --location westeurope

```
This will create a group called `resourceForStorageAccount`

## create a service plan

The full command for creating a service plan is this:

```
az appservice plan create --name myAppServicePlan --resource-group resourceForStorageAccount --sku B1 --is-linux
```
The value `B1` means we are on a basic service plan and `--is-linux` means we will get a linux container

We are using the `appservice` command above and the sub command `plan` to create our service plan. This should also give us a JSON response back with `provisioningState` key having value `Succeeded`

## create the web app
Below is the command for creating the web app. We will need to state the `resource group` also the `pricing plan` the `name` of the app, the version of Python and lastly how we will deploy it, which is `git deploy`:

```
az webapp create --resource-group resourceForStorageAccount --plan myAppServicePlan --name <app_name> --runtime "PYTHON|3.7" --deployment-local-git
```
A comment on the above command is that the `app name` will need to be globally unique. So let's try the followibf

```
az webapp create --resource-group resourceForStorageAccount --plan myAppServicePlan --name python-app-chris --runtime "PYTHON|3.7" --deployment-local-git
```

In the JSON response to this we are looking for two interesting pieces of information:

- the name of the git repository, look for key called `deploymentLocalGitUrl` . 
- the public url, look for a key called `defaultHostName`

What you've accomplished is an empty web app with git deployment enabled. What does that mean though?

## visit the web site
to visit the web site go to `http://<app name>.azurewebsites.net`

This should show you a default site looking like this:

![](/assets/Screen Shot 2019-01-10 at 22.35.25.png)

That seems to work, great !

## push the code to the web site
ok last step is to set up the git deploy properly and push the code and see the site being updated with your code. 
We set up the git deploy by typing the following:

```
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Now it's time to push the code so we simply type:

```
git push azure master
```
This will prompt you for your deployment users password. Just type it and your deployment are under way.

This will take a little while so go and have a warm beverage :)
It should finally dump out a long list of logs ending with:
![](/assets/Screen Shot 2019-01-10 at 22.44.29.png)

That means that your app deployed successfully and you can now go back to your app url and you should see this:
![](/assets/Screen Shot 2019-01-10 at 22.45.44.png)

## Manage changes and redeploy
This is quite easy now that we have git deploy activated. Let's do the following:

- make some changes to a file
- commit the changes
- push the code to our remote branch

### make some changes
Let's go into `application.py` and change it to this:
![](/assets/Screen Shot 2019-01-10 at 22.51.01.png)

### commit
We commit as we would normally do in git with:
```
git commit -am "changed text"
```

### push the code to our remote

Let's now push to our remote branch by typing:
```
git push azure master
```

This should generate a bunch of log entries and end with `deployment successful`. Let's verify that that is the case by going to our app url:
![](/assets/Screen Shot 2019-01-10 at 22.55.19.png)

That's looking good :)

## Summary
We have learned how to deploy an app to azure and even update it and redeploy. That's a lot in one article. Let's have a brief look at the steps we took to accomplish this:

- **downloaded** a sample app. Obviously in a real scenario we would have created our own code and not as in this case use a sample app with Python and Flask

- **created a `deployment user`** that we would need for our deployment. That also made us choose deployment type which we chose to be `git deployment`.

- **created a `resource group`**. This created a logical group for our app. This is a group we can reuse for other things like adding a database our something else the app might need to exist.

- **created a `service plan`**. This meant that we decided how we want to be billed for this as well as what kind of container we wanted the app to run in.

- **created the web site itself**, or rather a placeholder where our app could live.

- **pushing our code** to the newly created web site using git. We also showed how we could use git to update our web site and _redeploy_.

This taught us the basics of starting with web app development with azure and prepared us for future adventures. Stay tuned for more fun in the cloud :)     


