# Computer vision service


> WHY Cog Services: You need to decide whether to build or buy a solution. Building a sophisticated image processing and analysis engine is costly. One alternative is to use the Computer Vision API from Microsoft. 
![](/assets/blue-close-up-eye-685526.jpg)

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
As you can see above we pose a query and ask specifically for `key1`.
We can see our command worked by typing:

```
echo $key // should print `key1` value
```

## Talking to the API
Time has come to talk to the API. We will attempt to analyze an image. Let's have a look at what an API URL might look like :

```
https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=<...>&details=<...>&language=<...>
```
Above we are using the `analyze` method. There are also some parameters in there that we need to set such as `visualFeatures`, `details` and `language`
- **details**, this can have value `Landmarks` or `Celebrities`
- **visualFeatures**, this is about what kind of information you want back, The `Categories` option will categorize the content of the images like trees, buildings, and more. `Faces` will identify people's faces and give you their gender and age

There are some requirements on the images that you mean to process, namely:

- **Supported image formats**: JPEG, PNG, GIF, BMP.
- **Image file size** must be less than 4 MB.
- **Image dimensions** must be at least 50 x 50.

### Identifying landmarks
We need to set `visualFeatures=Categories,Description` and `details=Landmarks`. Then we need to set our key as the value of the header param `Ocp-Apim-Subscription-Key` ans 

> On some system the lib `jq` is not installed. We need it before we run our curl command below, `brew install jq`
```
curl "https://westeurope.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

curl "https://westeurope.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://static1.squarespace.com/static/51b3dc8ee4b051b96ceb10de/t/5a3d8165e2c4836cd19a9f7b/1513980264718/?format=2500w'}" \
| jq '.'




You should get back an answer looking like this:

```
{
  "categories": [
    {
      "name": "outdoor_mountain",
      "score": 0.99609375,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "snow",
      "outdoor",
      "mountain",
      "nature",
      "covered",
      "skiing",
      "man",
      "flying",
      "standing",
      "wearing",
      "side",
      "air",
      "slope",
      "jumping",
      "plane",
      "red",
      "hill",
      "riding",
      "people",
      "group",
      "yellow",
      "board",
      "doing",
      "airplane"
    ],
    "captions": [
      {
        "text": "a snow covered mountain",
        "confidence": 0.956279380622841
      }
    ]
  },
  "requestId": "3c99e0c5-0ab9-4baf-9b7d-b804a4e377f9",
  "metadata": {
    "width": 600,
    "height": 462,
    "format": "Jpeg"
  }
}
```

## Checking for inappropriate content
We can also call the APIO to determine wether our image contains adult/ inappropriate content. To accomplish that we only need to change the parameters.

Let's change `visualFeatures` to instead say `Adult,Description`. Our call will now look like this:

```
curl "https://westeurope.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```
The response coming back looks like this:

```
{
  "adult": {
    "isAdultContent": false,
    "isRacyContent": false,
    "adultScore": 0.0013711383799090981,
    "racyScore": 0.0046537225134670734
  },
  "description": {
    "tags": [
      "outdoor",
      "person",
      "posing",
      "photo",
      "grass",
      "group",
      "standing",
      "man",
      "people",
      "woman",
      "young",
      "holding",
      "dress",
      "white"
    ],
    "captions": [
      {
        "text": "a group of people posing for a photo",
        "confidence": 0.9906094762899921
      }
    ]
  },
  "requestId": "",
  "metadata": {
    "width": 600,
    "height": 462,
    "format": "Png"
  }
}
```

Let's highlight specifically on the `adult` property section:

```
"adult": {
  "isAdultContent": false,
  "isRacyContent": false,
  "adultScore": 0.0013711383799090981,
  "racyScore": 0.0046537225134670734
  }
```
We see the following parameters of interest:
- `isAdultContent` has the value `false`. 
- `isRacyContent` is also `false`
- `adultScore` is very low value close to 0. It would need to be closer to `1.0` to imply it's _adult content_. 
- `racyScore`, same thing here, a value close to `0`

## Generate thumbnail
Ok, so there is a thumbnail generating service. Why do we want that? Well let's look at the sales pitch for this service:

> Computer Vision first generates a high-quality thumbnail and then analyzes the objects within the image to determine the region of interest (ROI). Computer Vision then crops the image to fit the requirements of the region of interest. The generated thumbnail can be presented using an aspect ratio that is different from the aspect ratio of the original image, depending on your needs

Ok, so it seems to do a better job than a human finding the interesting part of an image, neat. Let's try it out.

Ok, let's have a look at what the URL looks like that we need to call to create the thumbnails we need:

```
https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=<...>&height=<...>&smartCropping=<...>
```
Ok first thing we see here is that we changed the name of the service we are going to use to `generateThumbnail`. Of course that means the parameters will change to. 

Parameters:

- **width**, required
- **height**, required
- **smartCropping**, Here is an example to why I would want this enabled - _with smart cropping enabled, a cropped profile picture would keep someone's face within the picture frame even when the picture has a different aspect ratio_



So an actual call to this service looks like this:

```
curl "https://westeurope.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```
This will create an image with the dimensions 100x100 and with the name `thumbnail.jpg`.

## Extract printed text from an image
The Computer Vision API contains some neat OCR algorithms that will be able to recognize text in our images.

The URL for calling this API looks like this:
```
https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr?language=<...>&detectOrientation=<...>
```
The service used is called `ocr`.

Parameters:
- **language**: The language code of the text to be detected in the image. The default value is unk,or unknown. This let's the service auto detect the language of the text in the image.
- **detectOrientation**: When true, the service tries to detect the image orientation and correct it before further processing, for example, whether the image is upside-down.

To perform the actual call we need to perform the following POST request:

```
curl "https://westeurope.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

The answer back looks like this:

```
{
  "language": "en",
  "textAngle": 0,
  "orientation": "Up",
  "regions": [
    {
      "boundingBox": "57,18,348,509",
      "lines": [
        {
          "boundingBox": "335,18,70,14",
          "words": [
            {
              "boundingBox": "335,18,70,14",
              "text": "Microsoft"
            }
          ]
        },
        {
          "boundingBox": "67,102,286,27",
          "words": [
            {
              "boundingBox": "67,102,60,27",
              "text": "NET"
            },
            {
              "boundingBox": "140,102,213,27",
              "text": "Microservices:"
            }
          ]
        },
        {
          "boundingBox": "57,139,239,29",
          "words": [
            {
              "boundingBox": "57,139,185,29",
              "text": "Architecture"
            },
            {
              "boundingBox": "253,139,43,29",
              "text": "for"
            }
          ]
        },
        {
          "boundingBox": "58,178,291,28",
          "words": [
            {
              "boundingBox": "58,178,209,28",
              "text": "Containerized"
            },
            {
              "boundingBox": "280,179,69,27",
              "text": ".NET"
            }
          ]
        },
        {
          "boundingBox": "57,216,188,37",
          "words": [
            {
              "boundingBox": "57,216,188,37",
              "text": "Applications"
            }
          ]
        },
        {
          "boundingBox": "277,465,93,10",
          "words": [
            {
              "boundingBox": "277,466,30,9",
              "text": "Cesar"
            },
            {
              "boundingBox": "310,466,14,9",
              "text": "de"
            },
            {
              "boundingBox": "328,465,9,10",
              "text": "la"
            },
            {
              "boundingBox": "341,466,29,9",
              "text": "Torre"
            }
          ]
        },
        {
          "boundingBox": "277,480,65,13",
          "words": [
            {
              "boundingBox": "277,480,17,10",
              "text": "Bill"
            },
            {
              "boundingBox": "298,480,44,13",
              "text": "Wagner"
            }
          ]
        },
        {
          "boundingBox": "277,495,71,9",
          "words": [
            {
              "boundingBox": "277,495,10,9",
              "text": "M"
            },
            {
              "boundingBox": "289,495,16,9",
              "text": "ike"
            },
            {
              "boundingBox": "308,495,40,9",
              "text": "Rousos"
            }
          ]
        },
        {
          "boundingBox": "277,515,110,12",
          "words": [
            {
              "boundingBox": "277,515,46,10",
              "text": "Microsoft"
            },
            {
              "boundingBox": "327,515,60,12",
              "text": "Corporation"
            }
          ]
        }
      ]
    }
  ]
}
```
The interesting part is in the `regions` key which is an array of found text occurrences. Let's have a look at one such item:

```
{
  "language": "en",
  "textAngle": 0,
  "orientation": "Up",
  "regions": [
    {
      "boundingBox": "57,18,348,509",
      "lines": [
        {
          "boundingBox": "335,18,70,14",
          "words": [
            {
              "boundingBox": "335,18,70,14",
              "text": "Microsoft"
            }
          ]
        }
        ]
    }
```
We see above from our response we are given the `orientation` for the picture and also `regions` which are all the different sections in the image. Looking deeper in the response we have `lines` which is an array that points to x number of instance where each instance looks like this:

```
{
  "boundingBox": "335,18,70,14",
  "words": [
    {
      "boundingBox": "335,18,70,14",
      "text": "Microsoft"
    }
  ]
}
```
Above we see that `words` that contains one instance per found word at a specific place.  

## Extract handwritten text
This is great to use when you have a lot of handwritten notes and want to know what they say. It's really good at recognizing hand written text.

Let's have a look at what the URL looks like to call this service:
```
https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=<...>
```
Above we are using the method `recognizeText` and we use the following parameters with it:

- `mode`, this has to have the value `HandWritten` or `Printed`

Calling this service therefore looks like this:

```
curl "https://westeurope.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

## Summary


