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

## Create a Coginitive Service account
we need a Cognitive Services account for the Computer Vision API
The command for creating the account is as follows:

```
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group [sandbox resource group name] \
--location [location]
```

