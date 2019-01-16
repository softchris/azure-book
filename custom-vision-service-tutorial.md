## Custom Vision Service Tutorial
The idea with using Custom Service is that you have a pre trained model already. However you may need the model to perform better in certain areas so this is where you add a specific type of images in a specific type of category to get it to recognize what you need.

Let's have a look at the below tutorial in which we learn to identify paintings from famous artists.

We will take the following steps:

- **Create** a Custom Vision Service project
- **Train** a Custom Vision Service model with tagged images
- **Test** a Custom Vision Service model
- **Call** your custom model's prediction endpoint over HTTP

## Create a Custom Vision Project

We need to go to the following URL https://www.customvision.ai/
Sign in and click `New Project`

You will be presented with the following modal:
![](/assets/Screen Shot 2019-01-16 at 16.47.25.png)

name the project `Artworks`, and make sure that `General` is selected in the `Domains` list. You can keep the `default settings` for `Project Types` and `Classification Types`. 

Select `Create project` to create our project.

## Upload tagged images
Click the `+` to the right of `Tags`:
![](/assets/Screen Shot 2019-01-16 at 16.50.40.png) 
Enter the title `painting`, like so:
![](/assets/Screen Shot 2019-01-16 at 16.51.30.png)

Repeat the steps to add `Picasso`, `Pollock`, and `Rembrandt`

So we now have this:
![](/assets/Screen Shot 2019-01-16 at 16.53.13.png)

Go to the following link to download the images of paintings:
> https://github.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/raw/master/cvs-resources.zip

Once you've downloaded the zip file look for the directory 
> Artists\Picasso
Click `Add images` and add all the images from that folder.

Select the tag `painting` and `Picasso` and complete the dialog:
![](/assets/Screen Shot 2019-01-16 at 20.26.24.png)

With seven Picasso images, the Custom Vision Service can do a decent job of identifying paintings by Picasso. But if you trained the model right now, it would only understand what a Picasso looks like, and it wouldn't be able to identify paintings by other artists. 

> The next step is to upload some paintings by another artist.

Let's do just that by selecting the directory `Artists\Rembrandt` and tag all images with `painting` and `Rembrandt`

Lastly let's get those Pollock paintings in there by selecting `Artists\Pollock` directory and give them the tag `painting` and `Pollock`

## Train the model
To train the mode it's just a few simple steps:
- hit `Train` button
- look at the stats from the training session
- add additional images, after the training has finished if you wish for it to be more exact

When it's finished it will have produced an `iteration` that you can click into and it should look like this:
![](/assets/Screen Shot 2019-01-16 at 20.38.46.png)

As you can see above two measures are being presented `Precision` and `Recall`.

Suppose the model was presented with three Picasso images and three from Van Gogh. 

Let's say it correctly identified two of the Picasso samples as "Picasso" images, BUT incorrectly identified two of the Van Gogh samples as Picasso. 

In this case, the `Precision` would be **50%**, since it identified two out of four images correctly. 

The `Recall` score would be **67%** since it correctly identified two of the three Picasso images correctly.

## Test the model
Now that we've trained our model, it's time to test it. We'll give the model new images and see how well it classifies it.

We will do the following:
- hit the button `Quick test`
- select an image to test
- examine the results
- tag the image with the correct tag, to tell the model what it was

The results should look something like this:
![](/assets/Screen Shot 2019-01-16 at 20.46.51.png)
Let's try to tag the image correctly by giving it the tags `Picasso`. To do that click the button `Predictions`. So close the dialog, click `Predictions` and select the image you just tested and add the tag.

To ensure our model seem properly trained select the file `FlowersTest.jpg` and ensure it gets a low score for being any of the famous painters. It should look something like so:
![](/assets/Screen Shot 2019-01-16 at 20.54.09.png)

## Call the model's prediction endpoint over HTTP
Ok so we trained our model, now it's time to reap the fruits of our labour. We will hit the our CustomVisionService API. The API will use our newly trained model as a reference when trying to find out what a sample image is.

Ok we will take the following steps:

- make the latest iteration default, so it knows what state of the model it should compare from, click `Performance` and then click `Make Default`
- retrieve the URL we need to hit, to use our model, click `Prediction Url`, this should produce a modal looking like this:
![](/assets/Screen Shot 2019-01-16 at 21.11.35.png)

As you can see there are two ways to talk the API, we can either give it an image URL, if it's an external image URL or if we select an image locally from our computer we should go with the option below. Also important is to set the `Prediction-Key`.

- **Prediction-Key**: This key has to be set as a header in all requests. That's what gives us access to the endpoint.
- **Request URL**: The dialog shows two different URLs. If we're posting an image URL, then use the first URL, which ends in `/url`. If we want to post a raw image in the body of our request, we use the second URL, which ends in `/image`.
- **Content-Type**: If we're posting a raw image, we set the body of the request to the binary representation of the image and the content type to `application/octet-stream`. If we're posting an image URL, we put that as JSON in the body and set the content type to `application/json`.

