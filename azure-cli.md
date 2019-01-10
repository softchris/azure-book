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


