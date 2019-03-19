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
specifically specify `returnFaceAttributes=emotion`

# Summary

## Further reading

[Mojifier LEARN module](https://docs.microsoft.com/en-us/learn/modules/replace-faces-with-emojis-matching-emotion/)