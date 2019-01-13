# Computer vision service

> WHY Cog Services: You need to decide whether to build or buy a solution. Building a sophisticated image processing and analysis engine is costly. One alternative is to use the Computer Vision API from Microsoft. 
We will do the following

- **Create** a Cognitive Services account.
- **Get** information about the visual content found in an image.
- **Generate** a thumbnail of an image.
- **Detect** and extract printed text from an image.
- **Detect** and extract handwritten text from an image.

## Calling the API

- get an API key
- make a POST call to the API
- parse the JSON response

Format of the URL

```
region.api.cognitive.microsoft.com/vision/v2.0/resource/[parameters]
```

- **region** - the region where you created the account, for example, westus.
- **resource** - the Computer Vision resource you are calling such as 
- **analyze**, describe, generateThumbnail, ocr, models, recognizeText, tag.

## Azure CLI
You can manage cognitive services through either UI or the Azure CLI

```
az cognitiveservices <command> // e.g `list`
```
Commands:

- **list**, List available Azure Cognitive Services accounts.
- **account show**, Get the details of an Azure Cognitive Services account.
- **account create**, Create an Azure Cognitive Services account.
- **account delete**, Delete an Azure Cognitive Services account.
- **keys list**, List the keys of an Azure Cognitive Services account.

## Create a Cognitive Service account
we need a Cognitive Services account for the Computer Vision API
The command for creating the account is as follows:

```
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group [resourceGroup] \
--location [location]
```
Ok so we need to create a `resource group` and decide on a `location`.

### Create a resource group
We can simply do that by typing
```
az group create \
    --name resourceforcogservices \
    --location westeurope

```

### Create the cog services account

```
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group resourceforcogservices \
--location westeurope

```
That should have create the `cog services account`.

## Verify account
We can easily verify that the account was created by typing:

```
az cognitiveservices account list \
--resource-group resourceforcogservices // returns array []
// OR
az cognitiveservices account show \
--name ComputerVisionService \
--resource-group resourceforcogservices // returns a specific account by name

```

### Get the keys

```
az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group resourceforcogservices
```

This retuns a list of keys that we can use to do our REST API call.

We can save down the first `key` in our response to a variable that we can refer to later, like so:

```
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group resourceforcogservices \
--query key1 -o tsv)
```
As you can see above we pose a query and ask specifically for `key1`
