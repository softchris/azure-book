# Mojifier

- **Classify** the emotion of any people in the image
- **Match** emotions to emojis
- **Replaces** faces in the image with emojis
- **Post** the updated image to Slack as a reply

Uses techs are

- Azure Functions
- Azure Cognitive Services ( [Face API](https://azure.microsoft.com/en-gb/services/cognitive-services/face/) )

## How do you map an emotion to an emoji?
[Euclidian distance](https://docs.microsoft.com/en-us/learn/advocates/replace-faces-with-emojis-matching-emotion/media/graph-2.png)

measuring 
- anger 
- contempt 
- disgust 
- fear 
- happiness 
- neutral
- sadness 
- surprise

### Making a POST call

```
https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion
```
specifically specify `returnFaceAttributes=emotion`, the payload in the POST call needs to be
```
{ "url": "<path to url>" }
```

You also need to specify a header, like so:
```
Ocp-Apim-Subscription-Key: <your-subscription-key>
```

The response looks like the following:
```
[
  {
    "faceRectangle": {
      "top": 207,
      "left": 198,
      "width": 229,
      "height": 229
    },
    "faceAttributes": {
      "emotion": {
        "anger": 0.001,
        "contempt": 0.014,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.306,
        "neutral": 0.675,
        "sadness": 0.003,
        "surprise": 0.001
      }
    }
  }
]
```
Essentially it's giving you two types of response back
- where the face is in the image
- what emotions where detected and how much of it on a sliding range of 0 to 1

The response is an array, one item for each detected face

# Summary

## Further reading

[Mojifier LEARN module](https://docs.microsoft.com/en-us/learn/modules/replace-faces-with-emojis-matching-emotion/)