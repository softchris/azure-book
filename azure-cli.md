# Azure CLI

The Azure CLI is a command-line program to connect to Azure and execute administrative commands on Azure resources. It runs on Linux, macOS, and Windows and allows administrators and developers to execute their commands through a terminal or command-line prompt

Example command that restarts a virtual machine:

```
az vm restart -g MyResourceGroup -n MyVm
```

Can be used in two different ways:

* installed locally
* used from a browser through the Azure Cloud Shell

Example of installing it for Mac:

```
brew update && brew install azure-cli
```

To sign in we type:

```
az login
```

typing the above will take you to a browser where you are asked to login. Once that is done you are good to go.

## Commands

Commands in the CLI are organized as _commands \_of \_groups_

Next up let's see what commands we have at our disposal:

* `az group`, for resource groups
* `az vm` , for virtual machines
* `az storage account`, for storage accounts
* `az keyvault`, for managing keyvault
* `az webapp`, for managing webapps
* `az sql server`, for managing sql server databases
* `az cosmosdb`, for managing cosmosdb

Searching for commands names containing the word `secret`, type the following:

```
az find -q secret
```
## Interactive mode
The CLI offers an interactive mode that automatically displays help information and makes it easier to select subcommands.

To use type:
```
az interactive
```

## Creating a storage account
Every storage account must belong to an Azure `resource group`. A `resource group` is a logical container for grouping your Azure services. When you create a storage account, you have the option to either create a `new resource group`, or `use an existing resource group`.

Let's first look at how to create a resource group:

```
az group create \
    --name resourceForStorageAccount \
    --location westeurope
```
The results of running it should look something like this:
![](/assets/Screen Shot 2019-01-10 at 01.01.22.png)
`provisioningState` should say `Succeeded`

Next up let's create the actual storage account:
```
az storage account create \
    --name storageaccount666 \
    --resource-group resourceForStorageAccount \
    --location westeurope \
    --sku Standard_LRS \
    --kind StorageV2
```
Storage account names needs to be lowercase
Also this one will answer with a long JSON response. Ensure the `provisioningState` says `Succeeded`

Let's have a look at our resource group in our Azure portal
![](/assets/Screen Shot 2019-01-10 at 01.11.59.png)
We can click `Resource groups` in the menu to the left and on the top middle we can search for our resource group `resourceForStorageAccount`. Doing so leads to a search list and as you can see from the image above we are looking at the detail page for the resource group. One thing that is listed is all the things associated with it. For now that's only our storage account `storageaccount666`. So now we clearly see the `azure cli` is working well.

Now that we are done we can either choose to let everything remain as it is or we can clean up our used resource. If we opt for the latter we should type the following:
```
az group delete --name resourceForStorageAccount
```

## Create a Web App and deploy it to Azure
Here we will look how we can create a webapp and deploy it to azure. 

We need to do the following:

- **download** a sample app
- **run the app** locally just to ensure it works
- **configure a deployment user**, this deployment user is required for `FTP` and `local Git deployment` to a web app
- **create a resource group**, this is needed if you need a new logical grouping for what we are about to do
- **create a service plan**, we will need this to specify how we will be charged for this and what kind of container we will create
- **create the web app**, we will need to run a command to create the actual web app and we will need to state here how we deploy it, we will choose git deploy, more on that below

### Download and run sample app

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

### Configure a deployment user
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

### create a resource group
If you want a new group, type the following:

```
az group create \
    --name resourceForStorageAccount \
    --location westeurope

```
This will create a group called `resourceForStorageAccount`

### create a service plan

The full command for creating a service plan is this:

```
az appservice plan create --name myAppServicePlan --resource-group resourceForStorageAccount --sku B1 --is-linux
```
The value `B1` means we are on a basic service plan and `--is-linux` means we will get a linux container

We are using the `appservice` command above and the sub command `plan` to create our service plan. This should also give us a JSON response back with `provisioningState` key having value `Succeeded`

### create the web app
Below is the command for creating the web app. We will need to state the `resource group` also the `pricing plan` the `name` of the app, the version of Python and lastly how we will deploy it, which is `git deploy`:

```
az webapp create --resource-group resourceForStorageAccount --plan myAppServicePlan --name <app_name> --runtime "PYTHON|3.7" --deployment-local-git
```



